---
title: "Claude로 원격 서버 작업하기: SSH 직접 실행, 소켓 재사용, 역방향 터널, Desktop SSH 세션"
date: 2026-11-12
weight: 11
tags: [claude-code, ssh, remote-control, server]
description: "Claude Code로 서버를 점검하고 조작하는 방법을 정리했다. ~/.ssh/config와 키로 ssh 명령을 보내는 방법, 열어 둔 SSH 연결(ControlMaster)과 역방향 터널을 쓰는 방법, 서버에서 Claude를 직접 돌리는 방법, Desktop SSH 세션, MCP·Ansible·SSM까지 비교했다. 상황별로 어떤 방법을 고를지와 추천 순서, 실습으로 확인한 설정과 보안 제한도 붙였다."
---

> 2026-11-12 기준으로 Claude Code 2.1.322와 [공식 문서](https://code.claude.com/docs/en/overview)를 바탕으로 정리했다. SSH 설정은 OpenSSH 10.3 컨테이너 두 대로 직접 실습해 확인했다. 실습에서 나온 오류 메시지도 그대로 옮겼다.

Claude에게 서버 작업을 맡기는 방법은 크게 두 갈래다. **Claude가 내 컴퓨터에서 돌면서 SSH로 명령을 보내는 방법**과 **Claude를 서버에서 직접 돌리는 방법**이다. 처음 떠올린 두 방법은 앞쪽에 속한다. 공식 문서와 커뮤니티 도구를 찾아보니 뒤쪽에도 쓸 만한 선택지가 여럿 있었다.

## 한눈에 보기

| 방법 | 실행 위치 | 연결 방향 | 맞는 경우 |
|---|---|---|---|
| 1. `~/.ssh/config` + 키로 `ssh 호스트 명령` | 내 컴퓨터 | 나 → 서버 | 서버 한두 대를 지켜보며 점검·수정 |
| 2-a. 열어 둔 SSH 연결 재사용(ControlMaster) | 내 컴퓨터 | 나 → 서버 | OTP·비밀번호 로그인이라 Claude가 직접 로그인하지 못할 때 |
| 2-b. 역방향 터널 | 내 컴퓨터(또는 중계 서버) | 서버 → 나 | 서버가 NAT·방화벽 뒤라 들어갈 수 없을 때 |
| 3. 서버에서 `claude` 실행(+ Remote Control) | 서버 | 나 → 서버 | 서버 안에서 오래 걸리는 작업, 휴대폰으로 이어 보기 |
| 4. Claude Desktop SSH 세션 | 서버(Desktop이 설치) | 나 → 서버 | 화면에서 편하게, 연결이 끊겨도 작업이 계속 돌기를 원할 때 |
| 5. VS Code Remote-SSH + Claude Code 확장 | 서버 | 나 → 서버 | 이미 VS Code로 원격 개발을 할 때 |
| 6. SSH MCP 서버 | 내 컴퓨터 | 나 → 서버 | 명령 분류·승인·감사 로그를 도구 단계에서 걸고 싶을 때 |
| 7. Ansible | 내 컴퓨터 | 나 → 서버 | 여러 대에 같은 변경을 반복할 때 |
| 8. SSM, Self-hosted environments | 서버 또는 내 망 안 | 서버 → 밖(아웃바운드만) | SSH 포트를 열 수 없을 때 |

## 추천: 대부분은 방법 1부터 시작한다

처음 Claude에게 서버를 맡긴다면 **방법 1을 기본으로 두고, 방법 1로 안 되는 상황이 생길 때 다른 방법을 더하는 것**을 권한다.

- **준비가 가장 적다.** SSH 키 하나면 되고 서버에 아무것도 설치하지 않는다.
- **설정이 한 곳에 모인다.** Claude가 내 컴퓨터에서 돌기 때문에 로그인 정보, `CLAUDE.md`, 권한 규칙이 모두 내 컴퓨터에 있다. 한 세션에서 여러 서버를 오갈 수도 있다.
- **서버가 마지막 경계가 된다.** `authorized_keys`의 `command=` 제한과 `sudoers`는 Claude Code 권한 설정을 잘못해도 서버에서 그대로 막는다. Claude를 서버에서 직접 돌리는 방법 3~5에서는 `sudoers`는 남지만, 명령 단위로 묶는 `command=` 제한은 쓸 수 없다.

OTP 서버면 2-a, 바깥에서 들어갈 수 없는 서버면 VPN이나 2-b를 더한다. 개발 서버에서 파일을 많이 고치거나 오래 걸리는 작업이면 방법 4(터미널만 쓰면 3)로 옮기고, 여러 대에 같은 변경을 하면 방법 7을 쓴다. 자세한 판단 기준은 [상황별로 고르기](#상황별로-고르기)에 정리했다.

## 먼저 알아 둘 점: Claude는 프롬프트에 답하지 못한다

Claude Code의 Bash 도구는 명령마다 새 프로세스로 실행하고, 앞 명령에서 `export`한 환경 변수는 다음 명령에 남지 않는다([Tools reference](https://code.claude.com/docs/en/tools-reference)). 명령 중간에 입력을 기다리는 화면도 Claude가 대신 칠 수 없다. 그래서 SSH가 사람에게 묻는 모든 순간이 걸림돌이 된다.

| 묻는 것 | 막히는 모습 | 해결 |
|---|---|---|
| 비밀번호, 키 암호 | 멈추거나 실패 | 키 인증 + `ssh-agent`. 에이전트는 `claude`를 띄우기 **전에** 켜 둔다(`SSH_AUTH_SOCK`이 상속돼야 한다) |
| 처음 보는 호스트 키 확인 | `Host key verification failed.` | 지문을 확인하고 `known_hosts`에 미리 넣어 둔다 |
| `sudo` 비밀번호 | `sudo: a password is required` | 필요한 명령만 `NOPASSWD`로 열고, Claude에게 `sudo -n`을 쓰게 한다 |
| `top`, `less`, `vim` 같은 화면 프로그램 | 출력이 깨지거나 끝나지 않음 | `top -bn1`, `journalctl --no-pager -n 100`, `systemctl status --no-pager` |

`BatchMode yes`를 켜 두면 SSH가 묻는 대신 바로 실패한다. 실습에서 키가 없는 접속은 0.03초 만에 `Permission denied (publickey,keyboard-interactive).`와 종료 코드 255로 끝났다. 멈춰서 기다리는 것보다 이쪽이 Claude에게 훨씬 다루기 쉽다.

## 방법 1: `~/.ssh/config`와 키로 ssh 명령 보내기

처음 떠올린 방법이고, 가장 단순하다. Claude는 `ssh web1 'df -h'`처럼 명령 한 줄을 보내고 결과를 읽는다.

- **잘 맞는 경우**: 상태 확인, 로그 읽기, 장애 원인 찾기처럼 명령을 보내고 결과를 읽는 작업. 서버에 아무것도 설치하지 않고, 한 세션에서 여러 서버를 오갈 수 있다.
- **맞지 않는 경우**: 서버 파일을 많이 고치는 작업. Claude의 Read·Edit 도구는 내 컴퓨터 파일에만 쓰이므로 서버 파일은 `ssh`로 `sed`나 `tee`를 보내 고쳐야 하고, 따옴표 실수가 나기 쉽다. 오래 걸리는 작업도 불편하다. Bash 도구가 명령 하나를 기다리는 시간은 기본 설정에서 최대 10분이고([Tools reference](https://code.claude.com/docs/en/tools-reference#timeout-and-output-limits)), 내 컴퓨터가 잠들면 연결도 끊긴다. 그런 작업은 서버에서 `tmux`나 `systemd-run`으로 띄우고 결과만 확인하게 하거나 방법 3·4를 쓴다.

### 1) Claude 전용 키와 계정을 만든다

```bash
# 내 컴퓨터
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_claude -C claude-ops
ssh-copy-id -i ~/.ssh/id_ed25519_claude.pub claude-ops@203.0.113.10
```

키에 암호를 걸었다면 `ssh-add ~/.ssh/id_ed25519_claude`로 에이전트에 올린다(macOS는 `--apple-use-keychain`을 붙이면 키 암호가 키체인에 저장된다). 서버에는 내 개인 계정이나 root가 아니라 `claude-ops` 같은 전용 계정을 만든다. 무엇을 실행했는지 sshd 로그에서 계정으로 가려낼 수 있고, 권한도 따로 줄 수 있다.

### 2) 호스트 키를 확인하고 등록한다

```bash
# 내 컴퓨터: 서버가 내미는 키의 지문을 본다
ssh-keyscan -t ed25519 203.0.113.10 | ssh-keygen -lf -

# 서버 콘솔(또는 이미 믿는 연결): 서버 자신의 지문을 본다
ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

두 지문이 같을 때만 `ssh-keyscan -t ed25519 203.0.113.10 >> ~/.ssh/known_hosts`로 넣는다. `StrictHostKeyChecking accept-new`로 처음 본 키를 자동으로 믿게 하는 방법도 있지만, 첫 연결을 가로채면 그 키를 믿게 된다. 이 확인 절차는 [Desktop 문서의 호스트 키 확인](https://code.claude.com/docs/en/desktop#check-a-remote-machines-host-key)과 같다.

### 3) `~/.ssh/config`에 별칭을 둔다

```text
Host web1
  HostName 203.0.113.10
  User claude-ops
  IdentityFile ~/.ssh/id_ed25519_claude
  IdentitiesOnly yes
  BatchMode yes
  ForwardAgent no
```

- `IdentitiesOnly yes`: 에이전트에 키가 여러 개 있어도 이 키만 쓴다.
- `ForwardAgent no`: 에이전트 전달을 켜면 서버의 root가 그 소켓으로 내 다른 키를 쓸 수 있다. Claude 작업용 연결에서는 끈다.

### 4) Claude에게 서버 규칙을 알려 준다

프로젝트의 `CLAUDE.md`에 적어 두면 세션마다 설명하지 않아도 된다.

```markdown
## 서버
- `web1`(운영): 읽기 명령만 쓴다. 바꿔야 하면 바꿀 내용을 먼저 보여 주고 승인을 받는다.
- `dev1`(개발): 자유롭게 작업해도 된다.
- 원격 명령은 `ssh <호스트> '<명령>'`으로 한 번에 하나씩 실행한다.
- 화면을 쓰는 명령 대신 `top -bn1`, `journalctl --no-pager -n 100`을 쓴다.
- `sudo`는 `sudo -n`으로만 쓴다.
```

### 5) Claude Code 권한 규칙은 넓게 열지 않는다

`Bash(ssh web1 *)`를 허용하면 `ssh web1` 뒤에 오는 **모든 원격 명령**이 묻지 않고 실행된다. 공식 문서는 `docker exec`, `devbox run`처럼 인자를 명령으로 실행하는 도구에 대해 같은 경고를 한다. `Bash(devbox run *)`가 `devbox run rm -rf .`까지 허용한다는 것이다([Configure permissions](https://code.claude.com/docs/en/permissions)). `ssh 호스트 명령`도 같은 구조다.

```json
{
  "permissions": {
    "allow": [
      "Bash(ssh web1 uptime)",
      "Bash(ssh web1 df -h)",
      "Bash(ssh dev1 *)"
    ],
    "deny": ["Read(~/.ssh/**)"]
  }
}
```

- 운영 서버는 읽기 명령만 정확히 적고, 나머지는 기본 모드에서 매번 묻게 둔다.
- `ask` 규칙은 더 좁은 `allow` 규칙보다 우선한다. 그래서 `"ask": ["Bash(ssh web1 *)"]`를 같이 넣으면 위의 `uptime` 허용도 묻게 된다.
- `Read(~/.ssh/**)` 거부는 Claude가 `cat`이나 Read 도구로 개인 키를 읽는 것을 막는다. ssh 프로세스가 키를 쓰는 데는 영향이 없다.
- 문서는 인자를 제한하려는 패턴이 깨지기 쉽다고 경고한다. 따옴표나 공백 같은 작은 차이로도 규칙이 맞지 않을 수 있다. 그러니 권한 규칙은 편의용으로 쓰고, **실제 제한은 서버에서 건다**.

### 6) 서버에서 키가 할 수 있는 일을 묶는다

`authorized_keys`의 키 앞에 옵션을 붙이면, 그 키로는 정해 둔 명령만 돌릴 수 있다. 읽기 전용 점검용 키를 따로 두는 데 쓴다.

```text
# 서버: /home/claude-ops/.ssh/authorized_keys
restrict,command="/usr/local/bin/ro-cmd",from="198.51.100.7" ssh-ed25519 AAAA... claude-ro
```

```sh
#!/bin/sh
# /usr/local/bin/ro-cmd: 허용 목록에 있는 읽기 명령만 실행한다
case "$SSH_ORIGINAL_COMMAND" in
  "uptime"|"df -h"|"free -m"|"uname -a") exec $SSH_ORIGINAL_COMMAND ;;
  *) echo "denied: $SSH_ORIGINAL_COMMAND" >&2; exit 1 ;;
esac
```

- `restrict`는 포트 전달, 에이전트 전달, X11, PTY를 한 번에 끈다.
- `command=`가 있으면 클라이언트가 보낸 명령은 실행되지 않고 `$SSH_ORIGINAL_COMMAND`로만 전달된다.
- `from=`은 접속을 받을 출발지 주소를 정한다.

실습 결과는 다음과 같다.

```text
$ ssh lab-ro uptime
 00:58:33 up 5 days,  5:30,  0 users,  load average: 0.57, 0.91, 0.52
$ ssh lab-ro 'rm -rf /tmp/x'
denied: rm -rf /tmp/x                 # 종료 코드 1
$ ssh lab-ro 'uptime; id'
denied: uptime; id                    # 명령을 이어 붙여도 통째로 거부
$ ssh -N -L 9999:localhost:22 lab-ro
channel 2: open failed: administratively prohibited: open failed
```

변경 작업이 필요하면 키를 하나 더 만든다. 그 키의 계정에는 `sudoers`로 필요한 명령만 연다(예: `claude-ops ALL=(root) NOPASSWD: /usr/bin/systemctl restart nginx`).

### 7) 샌드박스를 켰다면

Claude Code의 [샌드박스](https://code.claude.com/docs/en/sandboxing)를 켜면 일반 `ssh`, `scp`, `rsync`는 허용한 호스트로도 연결하지 못한다. 샌드박스 안의 명령은 프록시를 거쳐서만 나가는데 ssh는 프록시 변수를 따르지 않기 때문이다. 필요한 명령만 샌드박스 밖으로 빼되, 문서 예시처럼 승인 규칙을 같이 건다. 빼낸 명령은 `allowedDomains`와 상관없이 어느 호스트로든 나갈 수 있다는 점도 기억한다.

```json
{
  "sandbox": { "excludedCommands": ["ssh web1 *"] },
  "permissions": { "ask": ["Bash(ssh web1 *)"] }
}
```

## 방법 2: 열어 둔 SSH 연결로 명령 보내기

두 번째로 떠올린 방법인 "대상 서버에서 SSH 소켓을 열어 두고 그 소켓으로 연결한다"는 실제로 두 가지 형태로 쓰인다. 글자 그대로 서버에 `nc -l`이나 `socat`으로 셸 포트를 열면 인증도 암호화도 없는 bind shell이 된다. 그 방식은 쓰지 않는다.

- **2-a. ControlMaster**: 사람이 한 번 로그인해 둔 SSH 연결을 유닉스 소켓으로 열어 두고, Claude의 `ssh`가 그 소켓을 다시 쓴다. 소켓은 **Claude가 도는 컴퓨터**에 생긴다.
- **2-b. 역방향 터널**: **대상 서버가 먼저** Claude 쪽으로 SSH 연결을 걸고, Claude 쪽에 포트를 열어 둔다. Claude는 그 포트로 서버에 들어간다.

### 2-a. ControlMaster: 한 번 로그인하고 Claude가 이어 쓰기

OTP나 비밀번호를 물어서 Claude가 직접 로그인할 수 없는 서버에 맞는 방법이다.

- **잘 맞는 경우**: OTP, 비밀번호, 터치가 필요한 하드웨어 키로만 들어갈 수 있는 서버나 bastion. 키 인증 서버에서도 반복 접속을 빠르게 하는 용도로 쓴다.
- **맞지 않는 경우**: 사람 없이 돌아가야 하는 작업. 사람이 먼저 연결을 열어야 하고, `ControlPersist` 시간이 지나면 다시 열어야 한다.

```text
Host bastion-otp
  HostName 203.0.113.20
  User sangjoon
  ControlMaster auto
  ControlPath ~/.ssh/cm/%C
  ControlPersist 4h
```

```bash
mkdir -m 700 -p ~/.ssh/cm

# 사람이 터미널에서 한 번: OTP를 입력하고 백그라운드로 연결을 열어 둔다
ssh -o BatchMode=no -fN bastion-otp

# 이후 Claude가 보내는 명령은 소켓을 타고 다시 로그인하지 않는다
ssh bastion-otp 'uptime'

ssh -O check bastion-otp   # Master running (pid=2843)
ssh -O exit bastion-otp    # 작업이 끝나면 닫는다
```

- 설정에 `BatchMode yes`를 넣어 두었다면, 사람이 여는 첫 연결에만 `-o BatchMode=no`를 붙인다. 명령줄 옵션이 설정 파일보다 먼저 적용된다.
- `ControlPersist`는 마지막 사용 뒤 소켓을 유지할 시간이다. 이 시간 동안 같은 사용자로 도는 프로세스는 누구든 인증 없이 그 연결을 쓸 수 있다. Claude가 쓰라고 연 것이지만 다른 프로세스도 쓸 수 있으니 시간을 짧게 잡고, 폴더 권한은 700으로 둔다.
- **macOS의 경로 길이 제한**: 실습에서 소켓을 긴 임시 폴더에 두었더니 연결 자체가 거부됐다. `ControlPath`는 `%C`(40자 해시)처럼 짧은 이름으로 `~/.ssh/cm/` 같은 짧은 폴더에 둔다.

```text
ControlPath too long ('/private/tmp/.../sshlab/cm-a5b1cd08cf471726b659991de6cb8cbeac63a1c0' >= 104 bytes)
```

명령마다 새로 연결하지 않으니 응답도 빨라진다. Bash 도구는 명령마다 새 프로세스로 `ssh`를 띄우므로, 키 인증 서버에서도 `Host *`에 ControlMaster를 걸어 두면 반복 접속 비용이 줄어든다.

### 2-b. 역방향 터널: 서버가 먼저 연결을 건다

서버가 NAT나 방화벽 뒤에 있어 바깥에서 22번 포트로 들어갈 수 없을 때 쓴다. 서버가 나가는 연결을 걸고, 그 연결 위로 Claude 쪽 포트를 서버의 sshd에 이어 둔다.

- **잘 맞는 경우**: 집 서버, 사무실 NAS, 고객사 장비처럼 바깥에서 들어갈 수 없는 서버. 서버 쪽 방화벽에 들어오는 포트를 열지 않아도 된다.
- **맞지 않는 경우**: VPN을 이미 쓰고 있거나 깔 수 있는 환경. 중계 서버(hub)와 터널 서비스를 따로 관리해야 해서 VPN보다 손이 많이 간다.

```text
① web1 ──(ssh -R 2222:localhost:22)──▶ hub   # hub의 127.0.0.1:2222가 열린다
② hub의 Claude ──(ssh -p 2222 localhost)──▶ 터널 ──▶ web1의 sshd
```

**대상 서버**에서 터널을 띄운다. 끊겨도 다시 붙도록 systemd 서비스로 등록한다.

```ini
# /etc/systemd/system/claude-tunnel.service (대상 서버)
[Unit]
Description=Reverse SSH tunnel to Claude host
After=network-online.target
Wants=network-online.target

[Service]
ExecStart=/usr/bin/ssh -N -i /etc/ssh/tunnel_key \
  -o ExitOnForwardFailure=yes -o ServerAliveInterval=30 -o ServerAliveCountMax=3 \
  -R 2222:localhost:22 tunnel@hub.example.com
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

**Claude 컴퓨터(hub)**에서는 터널용 키가 포트 하나만 열 수 있게 묶는다.

```text
# hub: /home/tunnel/.ssh/authorized_keys
restrict,port-forwarding,permitlisten="localhost:2222",command="/bin/false" ssh-ed25519 AAAA... web1-tunnel
```

Claude가 쓸 별칭도 만든다. `HostKeyAlias`를 주면 `known_hosts`에 `[localhost]:2222`가 아니라 `web1` 이름으로 키가 저장된다.

```text
Host web1-rev
  HostName localhost
  Port 2222
  User claude-ops
  HostKeyAlias web1
  IdentityFile ~/.ssh/id_ed25519_claude
  IdentitiesOnly yes
  BatchMode yes
```

실습에서 확인한 점은 다음과 같다.

- **hub의 sshd가 원격 전달을 허용해야 한다.** Alpine 기본 설정은 `AllowTcpForwarding no`였고, 터널은 `Error: remote port forwarding failed for listen port 2222`로 실패했다. 뒤쪽에 `AllowTcpForwarding yes`를 덧붙여도 소용없었다. sshd는 처음 나온 값을 쓰기 때문이다. 기존 줄을 `AllowTcpForwarding remote`로 고친 뒤 `sshd -T | grep -i allowtcpforwarding`으로 확인한다.
- **열린 포트는 hub의 루프백에만 붙는다.** `GatewayPorts` 기본값 덕분에 `127.0.0.1:2222`와 `::1:2222`로만 열렸고, hub 밖에서는 이 포트로 들어올 수 없다.
- **`permitlisten`이 실제로 막는다.** 같은 키로 2223번을 요청하자 `remote port forwarding failed for listen port 2223`으로 거부됐고, 셸 접속은 `/bin/false`로 바로 끝났다.
- 터널을 거쳐도 web1의 sshd가 다시 인증한다. 방법 1의 전용 계정, 키 제한, `sudoers` 설정은 그대로 적용된다.

중계 서버를 두기 싫다면 WireGuard나 Tailscale 같은 VPN으로 서버와 Claude 컴퓨터를 같은 망에 넣는 방법도 있다. 그러면 방법 1을 그대로 쓸 수 있다.

## 방법 3: 서버에서 Claude를 직접 실행하기

명령을 SSH로 나르지 않고, Claude Code를 서버에 깔아 그 안에서 돌린다. 따옴표 처리나 접속 반복이 없고, 서버의 파일을 Read·Edit 도구로 직접 다룬다.

- **잘 맞는 경우**: 서버 안에서 몇십 분에서 몇 시간 걸리는 작업(빌드, 테스트, 데이터 처리), 서버 파일을 많이 고치는 작업, 진행 상황을 휴대폰으로 보고 싶을 때.
- **맞지 않는 경우**: 운영 서버. 계정이 할 수 있는 일은 Claude도 모두 할 수 있고, 명령 단위 제한(`command=`)을 걸 수 없다. 로그인 정보도 서버에 남는다.

```bash
ssh web1
curl -fsSL https://claude.ai/install.sh | bash
tmux new -s claude    # SSH가 끊겨도 세션이 남는다
claude
```

- **로그인**: 서버에는 브라우저가 없으니 로그인 화면에 나온 URL을 내 컴퓨터 브라우저로 열고, 받은 코드를 `Paste code here`에 붙여 넣는다([설치·로그인 문제 해결](https://code.claude.com/docs/en/troubleshoot-install)).
- **Remote Control**: 서버의 tmux 안에서 `claude remote-control`(`claude rc`)을 띄우면 휴대폰 Claude 앱이나 claude.ai/code에서 그 세션을 이어서 쓸 수 있다. 공식 문서도 원격 머신에서는 tmux나 screen 안에서 띄우라고 안내한다. Pro, Max, Ultra, Team, Enterprise 플랜에서 claude.ai 로그인으로만 쓸 수 있고 API 키는 지원하지 않는다. Bedrock·Vertex·Foundry를 쓰거나 `ANTHROPIC_BASE_URL`을 다른 곳으로 돌려도 쓸 수 없다([Remote Control](https://code.claude.com/docs/en/remote-control)).
- **주의**: Claude가 그 계정의 권한을 그대로 갖고, 로그인 정보도 서버의 `~/.claude`에 남는다. 공용 서버에서는 전용 계정으로 실행한다. 서버에서 `api.anthropic.com`으로 나가는 HTTPS도 열려 있어야 한다.

## 방법 4: Claude Desktop의 SSH 세션

Claude Desktop의 Code 탭은 SSH 연결을 환경으로 저장해 두고, 그 서버에서 세션을 돌린다([Desktop: SSH sessions](https://code.claude.com/docs/en/desktop#ssh-sessions)). 방법 3을 Desktop이 대신 해 주는 셈이다.

- **잘 맞는 경우**: 클라우드 VM이나 사내 개발 서버에서 하는 코드 작업. 설치와 업데이트를 Desktop이 맡고, 터미널 없이 앱에서 세션을 관리한다. 노트북을 덮어도 작업이 이어진다.
- **맞지 않는 경우**: 운영 서버(방법 3과 같은 이유). 서버 사용자마다 약 1GB가 들고, 연결을 끊은 뒤 얼마나 더 도는지는 로그인 만료 시점에 달려 있다.

- **연결**: 환경 선택기에서 **New environment → SSH**를 열고 **SSH host**에 `user@host`나 `~/.ssh/config`의 `Host` 이름을 넣는다. macOS·Linux에서는 내 컴퓨터의 OpenSSH 클라이언트로 접속하므로 `ProxyJump`, `ProxyCommand`, `IdentityFile`, `CertificateFile` 같은 설정이 그대로 적용된다.
- **자동 설치**: 처음 접속할 때 Desktop이 서버의 `~/.claude/remote/`에 도우미 프로그램과 Claude Code를 설치한다. 사용자마다 약 1GB가 필요하고 `sudo`는 쓰지 않는다. 서버에는 SFTP 서브시스템이 켜져 있어야 하고, 설치 폴더가 `noexec`로 마운트돼 있으면 안 된다.
- **실행 권한**: Claude Code와 그것이 띄우는 명령은 SSH로 로그인한 사용자로 돈다. 문서도 그 사용자의 `sudo` 규칙을 사람에게 주는 권한처럼 검토하라고 한다.
- **끊겨도 계속**: 노트북을 덮거나 Desktop을 종료해도 서버 쪽 작업은 계속된다. 다만 Claude 계정으로 로그인했다면 로그인 토큰이 만료돼 몇 분 만에 멈출 수도 있다고 문서가 경고한다. 밤새 돌릴 작업이면 컴퓨터를 켜 두거나 클라우드 세션을 쓴다.
- **전달은 꺼진다**: Desktop은 설정과 상관없이 `ForwardAgent`, `LocalForward`, `RemoteForward` 등을 끄고 접속한다. 서버에서 띄운 개발 서버를 보려면 직접 `ssh -N -L 3000:localhost:3000 devbox`로 포트를 연다.
- **설정 위치**: MCP 서버(`~/.claude.json`, `.mcp.json`)와 스킬(`~/.claude/skills/`)은 서버 쪽 것을 읽는다.
- **SSM·컨테이너**: `ProxyCommand`로 AWS Session Manager를 거치거나, `docker exec -i <컨테이너> /usr/sbin/sshd -i`로 컨테이너 안까지 들어가는 예시도 문서에 있다.

## 방법 5: VS Code Remote-SSH + Claude Code 확장

VS Code의 Remote-SSH로 서버에 붙은 창에서 Claude Code 확장을 쓰면, 확장은 그 원격 시스템 기준으로 동작한다. 공식 문서도 인증서 설정을 설명하면서, SSH 호스트에 연결된 창이라면 로컬 컴퓨터가 아니라 그 시스템의 `~/.claude/settings.json`을 고치라고 안내한다([VS Code](https://code.claude.com/docs/en/vs-code)). 이미 VS Code로 원격 개발을 한다면 따로 준비할 것이 거의 없다.

- **잘 맞는 경우**: 이미 VS Code Remote-SSH로 그 서버에서 개발하고 있을 때.
- **맞지 않는 경우**: 운영 서버(방법 3과 같은 이유). VS Code를 쓰지 않는다면 방법 4가 준비할 것이 적다.

## 방법 6: SSH MCP 서버

SSH 접속을 MCP 도구로 감싼 커뮤니티 서버가 여럿 있다. 그중 [tufantunc/ssh-mcp](https://github.com/tufantunc/ssh-mcp) v2는 명령을 실행하기 전에 분류하고, 역할 × 호스트 그룹으로 허용 여부를 정하고, 파괴적인 명령은 사람의 승인을 받고, 결정을 모두 감사 로그에 남긴다.

- **잘 맞는 경우**: 여러 사람이 같은 서버들에 Claude를 붙이고, 명령마다 승인과 감사 기록을 남겨야 할 때.
- **맞지 않는 경우**: 혼자 서버 몇 대를 다루는 경우. 방법 1의 서버 쪽 제한으로 충분하고, 공식 도구가 아닌 프로그램에 키를 맡길 이유가 적다.

```bash
npm install -g ssh-mcp
# 설정: ~/.config/ssh-mcp/config.toml (macOS는 ~/Library/Application Support/ssh-mcp/config.toml)
claude mcp add --transport stdio ssh-mcp -- ssh-mcp
```

README가 직접 경고하는 두 가지는 지킨다. **root 계정에 연결하지 않는다.** **운영 프로필에는 `auto` 승인을 쓰지 않는다.** 공식 도구가 아니니 코드를 확인한 뒤 키를 맡긴다. 방법 1보다 나은 점은 Bash 권한 규칙으로는 걸기 어려운 명령 단위 제한을 도구 안에서 건다는 것이다.

## 방법 7: 여러 대라면 Ansible

같은 변경을 여러 서버에 반복한다면 Claude에게 명령을 하나씩 보내게 하기보다 플레이북을 쓰게 한다. Ansible은 `~/.ssh/config`를 그대로 쓰므로 방법 1의 준비를 그대로 활용한다.

- **잘 맞는 경우**: 여러 대에 같은 변경을 하거나, 같은 변경을 나중에 다시 해야 할 때. 바뀔 내용을 `--check --diff`로 먼저 본다.
- **맞지 않는 경우**: 한 번 보고 끝낼 점검이나 장애 분석. 플레이북을 쓰는 품이 더 든다.

```bash
ansible-playbook -i inventory.ini site.yml --check --diff   # 무엇이 바뀔지 먼저 본다
ansible-playbook -i inventory.ini site.yml                  # 검토한 뒤 적용
```

플레이북은 파일로 남으니 리뷰하고 커밋할 수 있다. 같은 작업을 다시 돌려도 결과가 같다는 점도 운영 서버에서는 큰 장점이다.

## 방법 8: SSH 포트를 열지 않는 경로

- **잘 맞는 경우**: AWS에서 22번 포트를 열 수 없을 때(SSM), 조직 정책상 바깥에서 들어오는 연결을 받을 수 없을 때(Self-hosted environments).
- **맞지 않는 경우**: SSM은 AWS 전용이다. Self-hosted environments는 저장소를 받아 하는 코드 작업용이라 서버 운영에는 맞지 않는다.

자세한 내용은 다음과 같다.

- **AWS Systems Manager**: 인바운드 22번 없이 SSM 에이전트의 아웃바운드 연결로 접속한다. `~/.ssh/config`에 `ProxyCommand sh -c "aws ssm start-session --target %h --document-name AWS-StartSSHSession --parameters 'portNumber=%p'"`를 두면 방법 1, 4를 그대로 쓸 수 있다(Desktop 문서의 예시).
- **Self-hosted environments**: Team·Enterprise 플랜의 공개 베타다. claude.ai에서 시작한 클라우드 세션을 내 망 안의 runner가 받아 실행한다. runner는 `api.anthropic.com`에 아웃바운드 HTTPS로만 붙고, 문서 표현으로는 "Anthropic never connects into your network"다([Self-hosted environments](https://code.claude.com/docs/en/self-hosted-environments)).

## 상황별로 고르기

### 질문 세 개로 좁힌다

1. **바깥에서 SSH로 들어갈 수 있는가?** 못 들어가면 연결 경로부터 만든다. VPN(Tailscale, WireGuard)을 깔 수 있으면 VPN, 아니면 방법 2-b, AWS라면 방법 8의 SSM을 쓴다. 경로가 생기면 다음 질문으로 간다.
2. **키로 로그인할 수 있는가?** OTP나 비밀번호로만 된다면 방법 2-a로 사람이 연결을 열어 두고, 그 위에서 방법 1처럼 쓴다.
3. **무엇을 시키는가?**
   - 점검, 로그 확인, 장애 원인 찾기: 방법 1
   - 서버 안 코드나 설정 파일을 많이 고치는 개발 작업: Desktop을 쓰면 방법 4, VS Code를 쓰면 방법 5, 터미널만 쓰면 방법 3
   - 10분 넘게 걸리는 작업: 방법 3(tmux + Remote Control) 또는 방법 4
   - 여러 대에 같은 변경: 방법 7
   - 여러 사람이 쓰면서 명령 승인과 감사 기록이 필요한 경우: 방법 6

### 서버 성격으로 정한다

같은 작업이라도 서버가 망가졌을 때 피해가 얼마나 큰지에 따라 고를 방법이 달라진다.

| 서버 | 추천 | 이유 |
|---|---|---|
| 개발 서버, 클라우드 VM(다시 만들 수 있음) | 방법 4나 3으로 서버에서 Claude를 돌린다. 가벼운 확인은 방법 1에 `Bash(ssh dev1 *)` 허용 | 서버 파일을 Read·Edit 도구로 바로 다뤄 편하다. 잘못돼도 다시 만들면 된다 |
| 운영 서버 | 점검은 방법 1의 읽기 전용 키. 변경은 방법 7로 `--check --diff`를 본 뒤 적용하거나, 방법 1에서 명령마다 승인 | 서버 쪽 키 제한이 Claude가 넘을 수 없는 경계가 된다. Claude를 운영 서버에 설치하지 않는다 |
| 집·사무실 서버, 고객사 장비 | VPN + 방법 1, 안 되면 방법 2-b | 서버에 들어오는 포트를 열지 않는다 |
| OTP를 거치는 서버 | 방법 2-a | 사람이 로그인하고 Claude는 그 연결만 쓴다 |
| AWS EC2(22번 닫힘) | 방법 8의 SSM `ProxyCommand` + 방법 1 | 들어오는 포트 없이 방법 1을 그대로 쓴다 |

### 처음 도입하는 순서

1. 읽기 전용 키(`restrict,command=`)로 방법 1을 붙이고 점검만 시킨다. 이 단계에서는 Claude가 실수해도 서버가 바뀌지 않는다.
2. `~/.ssh/config`의 `Host *`에 ControlMaster를 짧은 `ControlPersist`로 걸어 반복 접속을 줄인다.
3. 개발 서버는 `Bash(ssh dev1 *)`를 허용하거나 방법 4로 옮겨 자유롭게 쓰게 한다.
4. 운영 서버를 바꿔야 할 때가 오면 변경용 키와 좁은 `sudoers`를 따로 만든다. 같은 변경이 반복되면 Ansible 플레이북으로 옮긴다.

### 피하는 조합

- **root(또는 `sudo ALL`) 계정에 `Bash(ssh prod *)` 허용**: Claude가 운영 서버에서 무엇이든 묻지 않고 실행한다.
- **운영 서버에서 방법 3·4·5**: 계정 권한이 곧 Claude의 권한이 되고, 로그인 정보도 서버에 남는다.
- **`nc`·`socat`으로 연 셸 포트**: 인증도 암호화도 없다.
- **운영 프로필에 ssh-mcp `auto` 승인**: README가 직접 금지한다.
- **공용 컴퓨터에서 긴 `ControlPersist`**: 같은 사용자로 도는 다른 프로세스가 로그인 없이 그 연결을 쓴다.

## 보안 체크리스트

- [ ] 서버에 Claude 전용 계정을 둔다. root나 내 개인 계정을 쓰지 않는다.
- [ ] 전용 키를 쓰고 `authorized_keys`에 `restrict`, `command=`, `from=`을 건다.
- [ ] `sudo`는 필요한 명령만 `NOPASSWD`로 연다. `ALL`은 열지 않는다.
- [ ] `ForwardAgent`는 끈다.
- [ ] 호스트 키 지문을 확인한 뒤 `known_hosts`에 넣는다.
- [ ] `Bash(ssh 호스트 *)`처럼 넓은 허용 규칙은 개발 서버에만 쓴다. `Read(~/.ssh/**)`는 거부한다.
- [ ] ControlMaster 소켓은 `ControlPersist`를 짧게 하고, 폴더 권한을 700으로 둔다.
- [ ] 역방향 터널 키는 `permitlisten`으로 포트 하나만 열게 한다.
- [ ] 누가 무엇을 했는지는 서버의 sshd 로그와 셸 기록으로 확인한다.

## 참고 자료

- [Claude Code Desktop: SSH sessions](https://code.claude.com/docs/en/desktop#ssh-sessions)
- [Remote Control](https://code.claude.com/docs/en/remote-control)
- [Configure permissions](https://code.claude.com/docs/en/permissions)
- [Sandboxing: git over SSH, non-HTTP tools](https://code.claude.com/docs/en/sandboxing)
- [Tools reference: Bash 도구](https://code.claude.com/docs/en/tools-reference)
- [Self-hosted environments](https://code.claude.com/docs/en/self-hosted-environments)
- [tufantunc/ssh-mcp](https://github.com/tufantunc/ssh-mcp)
- OpenSSH 매뉴얼: `man ssh_config`(ControlMaster, ControlPath, ControlPersist), `man sshd`(AUTHORIZED_KEYS FILE FORMAT의 `restrict`, `command`, `permitlisten`), `man sshd_config`(AllowTcpForwarding, GatewayPorts)
