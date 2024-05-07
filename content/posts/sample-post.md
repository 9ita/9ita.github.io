---
author: "hdl"
title: "EMS Compact WIKI For Beginners"
image: ""
draft: true
date: 2024-05-07
description: "EMS Compact WIKI For Beginners"
tags: ["document"]
archives: ["2024/05"]
---

# EMS Compact WIKI For Beginners

해당 문서는 eActive Study Club을 위해 작성된 문서로 솔루션 기술서가 아니오니 참고만 하시길 바랍니다.

## 1. 시작하기

### 1.1 Introduction

- EMS는 무엇입니까?

> EMS는 eLink 솔루션 패키지의 한 프로젝트로, 온라인 거래 엔진 및 배치와 일괄 거래 엔진의 서비스 설정과 실시간 거래를 모니터링하는 툴입니다. 관리자 및 사용자는 EMS에서 제공되는 화면을 통해 거래 서비스를 구축하고 설정하며, 화면을 통해 모니터링 할 수 있습니다.

- Main Concepts and Terminology

> EMS의 화면과 기능을 이해하기 위해서는, 거래 서비스에 대한 이해가 전제로 필요합니다. 다음은 

  - 역할 분리를 통해 역할별 사용자의 화면권한 분리
  - APP Code 권한관리를 통해 사용자 계정별 자원권한 분리
  - 여러 타입의 서비스패키지를 하나의 EMS로 통합 관리
  - (A-A)의 HA구성을 제공

여 높은 가용성을 제공

  - 관리자 권한으로 엔진 프로그램의 재기동 필요없이 실시간 거래의 Rule 재반영
  - 실시간 각 거래엔진이 제공하는 여러가지 상태 모니터링
  - 
  - 

### 1.2 Quick Start

### 1.3 Other Versions

### 1.4 With Docker

## 2. Menus

## 3. Implementations

### 3.1 Configuration

- web.xml
- context.xml
- applicationContext.xml
- applicationContext-datasource-xxx.xml
- applicationContext-jdbc-xxx.xml
- applicationContext-jpa.xml
- applicationContext-schedule.xml
- applicationContext-udpip.xml
- springapp-servlet.xml

### 3.2 Log
- logback.xml

### 3.3 Filter
- 

### 3.4 AppInitializer

### 3.5 Interceptor

### 3.6 AOP

### 3.7 iBatis

### 3.8 JPA

### 3.9 Scheduler

### 3.9 Excel Download

### 3.10 Resource Deploy

## 4. User Interfaces

### 4.1 Common Scripts script.js, css

### 4.2 LocaleMessage

### 4.3 View Controller 동작 원리와 Menu 권한분리를 위한 설계

### 4.4 jExcel

### 4.5 jqGrid

### 4.6 fetch API & download

## 5. Operations

### 5.1 Command Class In Engine Project

### 5.2 AgentUtilService 

TSEAISY02

### 5.3 WebAgent Servlet

## 6.1 Security

### 6.2 Seed.java

enc, dec

## 7. Monitoring

### Bean ioAcceptor UDP 10800

### emsClient

### Configuration in Engine Program

## ETC

### Testing

### 

SSL지원?

