# Python Automation

반복되는 화면 작업과 데이터 정리를 줄이기 위한 **Windows 데스크톱 자동화 프로그램 모음**입니다.

> 소스코드는 private입니다. 이 저장소는 채용·협업용 소개 문서입니다.  
> 계정, 서버 주소, 접속 정보, 고객 데이터는 적지 않습니다.

Python, Tkinter, Playwright, MySQL. 필요한 도구는 PyInstaller로 Windows 실행 파일로 묶습니다.

[기능 상세](docs/FEATURES.md) · [아키텍처](docs/ARCHITECTURE.md)

---

## 한눈에 보기

| | |
| --- | --- |
| **형태** | Windows 데스크톱 GUI |
| **언어** | Python |
| **화면** | Tkinter |
| **브라우저 작업** | Playwright (Chromium) |
| **계정 확인** | MySQL, 사용 기간, 세션 |
| **배포** | PyInstaller 실행 파일 + 브라우저 런타임 |

```text
운영자
  │
  ▼
작업 콘솔 (Tkinter) ── 시작 / 중지 / 로그
  │
  ▼
진입점 ── 사용 권한 확인 ── 작업 스레드
  │
  ▼
브라우저 실행 (Playwright) ── 로컬 설정 · 실행 로그
```

---

## 무엇을 묶었나

같은 패턴의 프로그램을 업무 단위로 나눠 두었습니다. 화면에서 작업을 고르고, 브라우저가 페이지를 연 뒤, 결과를 로그로 남깁니다.

| 프로그램 | 형태 | 하는 일 |
| --- | --- | --- |
| Place 작업 콘솔 | GUI 앱 | 장소 페이지 작업을 설정에 맞춰 실행하고 기록 |
| Blog 작업 콘솔 | GUI 앱 | 블로그 페이지 작업을 설정에 맞춰 실행하고 기록 |
| 목록 순회 도구 | GUI 스크립트 여러 개 | 로그인 후 대상 목록을 순서대로 열어 상태를 확인 |
| 스토어 페이지 확인 | 작은 GUI 앱 | 상품 페이지를 열고 화면 로그를 남김 |

Blog 작업 콘솔은 같은 구조의 작업본이 하나 더 있습니다. 역할은 같고, 진입점과 설정 위치만 다릅니다.

목록 순회 도구는 패키지로 나누지 않고, 도구마다 파일 하나로 GUI와 실행 로직을 같이 둡니다. 스토어 페이지 확인은 사용 권한 DB 없이 바로 실행하는 작은 도구입니다.

---

## 공통으로 들어 있는 것

- 작업 시작 / 중지와 진행 로그
- 로컬 JSON 설정 (작업 목록, 일정)
- Playwright로 Chromium을 띄워 페이지를 열고 입력을 넣음
- 실행 파일로 묶어 Python이 없는 PC에서도 실행
- 접속 정보는 코드에 두지 않고 환경 변수로 읽음

Place / Blog / 목록 순회 도구는 실행 전에 MySQL로 계정을 확인하고, 사용 기간과 세션을 맞춥니다. 비밀번호는 해시와 비교합니다.

---

## 기술

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat-square&logo=playwright&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)

Tkinter · PyInstaller · bcrypt

---

## 폴더가 나뉘는 방식

```text
작업 콘솔 (Place / Blog)
  main.py          로그인, 사용 기간, 세션, GUI 진입
  gui/             Tkinter 화면
  core/            브라우저 컨텍스트, 페이지 작업, 로그
  config/          작업 목록, 일정

목록 순회 도구
  도구별 스크립트 1개 + 공통 로그인

스토어 페이지 확인
  main.py          작은 입력 화면
  core/            페이지를 열고 로그를 남김
```

실행 흐름은 [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md), 화면과 설정은 [docs/FEATURES.md](docs/FEATURES.md)에 있습니다.

---

## 문서 범위

이 저장소에는 소스코드를 올리지 않습니다.  
서버 주소, 계정, 비밀번호, 고객 작업 목록, 실행 중 남은 로그도 포함하지 않습니다.

채용·협업·외주 확인이 필요하면 상담 후 공개 가능한 범위만 따로 공유합니다.
