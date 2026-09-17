---
title: "채널(chan): 버퍼, 블로킹, select, 주의점"
date: 2026-09-17
weight: 2
tags: [golang, go, channel, concurrency]
description: "Go 채널의 기본 개념과 종류, 언버퍼드·버퍼드 채널의 블로킹 동작, select를 이용한 논블로킹 수신, 데드락과 고루틴 누수 같은 주의사항을 정리한다."
---

## 기본 개념

### 타입 안전한 파이프

```go
ch := make(chan int) // int 타입 채널
ch <- 42 // 값 전송
value := <-ch // 값 수신
```

### 동기화 프리미티브

- 채널은 데이터 전달과 동시에 동기화 역할
- 고루틴 간 안전한 통신 보장

## 채널의 종류

### 버퍼 크기에 따른 분류

```go
// 언버퍼드 채널 (동기적)
ch1 := make(chan int)

// 버퍼드 채널 (비동기적)
ch2 := make(chan int, 10)
```

### 방향성에 따른 분류

```go
// 양방향 채널
var ch chan int

// 송신 전용
var sendOnly chan<- int

// 수신 전용
var recvOnly <-chan int
```

## 언버퍼드 채널 특성

### 동기적 동작

- 송신자와 수신자가 동시에 준비될 때까지 블로킹
- 핸드셰이크 방식의 통신

```go
ch := make(chan int)

go func() {
    ch <- 42 // 수신자가 있을 때까지 블로킹
}()

value := <-ch // 송신자가 있을 때까지 블로킹
```

### 동기화 역할

```go
done := make(chan bool)

go func() {
    // 작업 수행
    done <- true // 완료 신호
}()

<-done // 작업 완료까지 대기
```

## 버퍼드 채널 특성

### 비동기적 동작

```go
ch := make(chan int, 3)

ch <- 1 // 블로킹 안됨
ch <- 2 // 블로킹 안됨
ch <- 3 // 블로킹 안됨
ch <- 4 // 버퍼 가득참, 블로킹됨
```

### 큐(Queue) 역할

- FIFO (First In, First Out) 순서
- 생산자-소비자 패턴에 유용

## 채널 상태와 동작

### 열린 채널 vs 닫힌 채널

```go
ch := make(chan int)

// 채널 닫기
close(ch)

// 닫힌 채널에서 수신
value, ok := <-ch // ok는 false, value는 zero value

// 닫힌 채널로 송신 (패닉 발생)
// ch <- 1  // 패닉!
```

### nil 채널의 특성

```go
var ch chan int // nil 채널

// nil 채널 연산은 영원히 블로킹
// <-ch   // 영원히 블로킹
// ch <- 1 // 영원히 블로킹
```

## 메모리 모델과 동기화

### 메모리 가시성 보장

- 채널 송신 이전의 모든 메모리 연산이 수신 이후에 보임
- 명시적인 동기화 없이도 안전한 데이터 공유

```go
var data int

go func() {
    data = 42 // happens-before
    ch <- true // 송신
}()

<-ch // 수신
fmt.Println(data) // 42가 보장됨
```

## 고급 패턴

### Select 문과 함께

```go
select {
case msg1 := <-ch1:
    // ch1에서 수신
case msg2 := <-ch2:
    // ch2에서 수신
case <-timeout:
    // 타임아웃
default:
    // 논블로킹 동작
}
```

### 범위 기반 반복

```go
ch := make(chan int)

go func() {
    for i := 0; i < 5; i++ {
        ch <- i
    }
    close(ch) // 반드시 닫아야 함
}()

for value := range ch {
    fmt.Println(value) // 0, 1, 2, 3, 4
}
```

## 성능 특성

### 경량성

- 채널 자체는 매우 가벼운 자료구조
- 고루틴 간 컨텍스트 스위칭 비용 최소화

### 스케줄링 효율성

- Go 런타임이 채널 대기 상태의 고루틴을 효율적으로 스케줄링
- OS 스레드 블로킹 없이 고루틴만 블로킹

## 주의사항

### 데드락 방지

```go
// 데드락 상황
ch := make(chan int)
ch <- 1 // 수신자 없어서 영원히 블로킹
```

### 채널 누수 방지

```go
// 고루틴 누수 가능성
go func() {
    <-ch // 채널이 닫히지 않으면 고루틴이 누수됨
}()
```

### 적절한 버퍼 크기

- 너무 큰 버퍼: 메모리 낭비
- 너무 작은 버퍼: 불필요한 블로킹

채널은 Go의 "메모리를 공유하여 통신하지 말고, 통신하여 메모리를 공유하라"는 철학을 구현하는 핵심 도구입니다. 올바르게 사용하면 안전하고 효율적인 동시성 프로그래밍이 가능합니다.

## 블로킹 동작 상세

### 언버퍼드 채널의 경우

```go
ch := make(chan int)

go func() {
    fmt.Println("수신 시작")
    value := <-ch // 여기서 블로킹됨
    fmt.Println("수신 완료:", value)
}()

time.Sleep(2 * time.Second)
ch <- 42 // 2초 후 값 전송
```

### 버퍼드 채널의 경우

```go
ch := make(chan int, 2)
ch <- 1
ch <- 2 // 버퍼에 값이 있음

go func() {
    value1 := <-ch // 즉시 수신 (블로킹 안됨)
    value2 := <-ch // 즉시 수신 (블로킹 안됨)
    value3 := <-ch // 버퍼 비어있음, 블로킹됨
}()
```

## 블로킹의 특징

### 고루틴만 블로킹, OS 스레드는 블로킹 안됨

```go
go func() {
    <-ch // 이 고루틴만 블로킹
}()

go func() {
    // 다른 고루틴은 정상 실행
    fmt.Println("다른 고루틴 실행 중")
}()
```

### Go 스케줄러가 관리

- 블로킹된 고루틴은 스케줄링에서 제외
- 채널에 값이 오면 자동으로 깨어남
- OS 스레드는 다른 실행 가능한 고루틴으로 전환

## 실제 예제

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch := make(chan string)

    // 고루틴 1: 수신 대기
    go func() {
        fmt.Println("고루틴 1: 수신 대기 중...")
        msg := <-ch // 블로킹됨
        fmt.Println("고루틴 1: 받은 메시지:", msg)
    }()

    // 고루틴 2: 수신 대기
    go func() {
        fmt.Println("고루틴 2: 수신 대기 중...")
        msg := <-ch // 블로킹됨
        fmt.Println("고루틴 2: 받은 메시지:", msg)
    }()

    time.Sleep(1 * time.Second)
    fmt.Println("메인: 첫 번째 메시지 전송")
    ch <- "Hello"

    time.Sleep(1 * time.Second)
    fmt.Println("메인: 두 번째 메시지 전송")
    ch <- "World"

    time.Sleep(1 * time.Second)
}
```

**출력:**

```text
고루틴 1: 수신 대기 중...
고루틴 2: 수신 대기 중...
메인: 첫 번째 메시지 전송
고루틴 1: 받은 메시지: Hello
메인: 두 번째 메시지 전송
고루틴 2: 받은 메시지: World
```

## 논블로킹 수신 방법

### select with default

```go
select {
case value := <-ch:
    fmt.Println("수신:", value)
default:
    fmt.Println("수신할 값 없음")
}
```

### 타임아웃과 함께

```go
select {
case value := <-ch:
    fmt.Println("수신:", value)
case <-time.After(1 * time.Second):
    fmt.Println("타임아웃")
}
```

## 주의사항

### 메인 고루틴 종료 시

```go
func main() {
    ch := make(chan int)

    go func() {
        <-ch // 블로킹됨
    }()

    // 메인 고루틴이 종료되면 프로그램 전체 종료
    // 위 고루틴은 영원히 대기하다가 같이 종료됨
}
```

### 고루틴 누수 방지

```go
func worker() {
    ch := make(chan int)

    go func() {
        for {
            select {
            case value := <-ch:
                // 작업 수행
            case <-ctx.Done(): // 컨텍스트로 종료 신호
                return
            }
        }
    }()
}
```

정리하면, 채널에서 값을 수신할 때 해당 고루틴은 블로킹되지만, 이는 **Go 런타임 레벨의 블로킹**이므로 OS 스레드나 다른 고루틴의 실행에는 영향을 주지 않습니다.
