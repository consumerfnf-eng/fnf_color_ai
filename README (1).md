# F&F Color AI — Color Intelligence System

F&F 컨슈머전략팀 내부용 컬러 인텔리전스 툴.  
트렌드 데이터(WGSN · Peclers · Newsletter) + 시장 데이터(경쟁사 · MLB 판매)를 통합해  
시즌별 컬러 팔레트를 분석하고 제안하는 단일 HTML 앱입니다.

---

## 실행 방법

별도 설치 없음. 파일 하나로 동작합니다.

```
FnF_Color_AI_v2.html  ←  브라우저로 열기
```

1. `FnF_Color_AI_v2.html` 다운로드
2. 크롬(Chrome) 브라우저로 열기
3. 비밀번호 입력 → 앱 진입

> **비밀번호 변경 방법**  
> 파일에서 `cst2026!` 를 검색 → 원하는 비밀번호로 교체

---

## 데이터 연동 구조

### Google Sheets 1개로 전체 관리

스프레드시트 하나 안에 탭 5개를 유지합니다.

```
[ newsletter ] [ wgsn ] [ peclers ] [ market ] [ mlbSales ]
```

앱 사이드바에서 시즌 선택 → **LOAD DATA** 버튼 → 해당 시즌 데이터만 자동 로드.

### Sheets URL 교체 방법

`FnF_Color_AI_v2.html` 파일 안에서 아래 부분을 찾아 URL을 교체합니다.

```javascript
const SHEETS_URLS = {
  newsletter:  'https://docs.google.com/spreadsheets/d/e/.../pub?gid=XXXX&output=csv',
  wgsn:        'https://docs.google.com/spreadsheets/d/e/.../pub?gid=XXXX&output=csv',
  peclers:     'https://docs.google.com/spreadsheets/d/e/.../pub?gid=XXXX&output=csv',
  competitor:  'https://docs.google.com/spreadsheets/d/e/.../pub?gid=XXXX&output=csv',
  mlbSales:    'https://docs.google.com/spreadsheets/d/e/.../pub?gid=XXXX&output=csv',
};
```

> **pub URL 만드는 법**  
> 구글 시트 열기 → 파일 → 웹에 게시 → 탭 선택 → CSV → 게시 → URL 복사

---

## 시트 작성 규칙

각 탭의 컬럼 구조를 반드시 아래와 동일하게 맞춰야 합니다.  
**첫 행은 헤더, 두 번째 행부터 데이터.**

---

### 📄 newsletter 탭

| 컬럼 | 형식 | 예시 | 설명 |
|---|---|---|---|
| `season` | 텍스트 | `27FW` | 시즌 코드 — LOAD DATA 필터 기준 |
| `no` | 숫자 | `1` | 순번 |
| `hex` | `#RRGGBB` | `#1E4477` | 컬러 HEX 코드 |
| `pantone` | 텍스트 | `19-4057 TCX` | Pantone 코드 (없으면 빈칸) |
| `source_file` | 텍스트 | `Forecast_AW27.pdf` | 원본 파일명 |

```
season	no	hex	        pantone	        source_file
27FW	1	#1E4477	        19-4057 TCX	AW27_Forecast.pdf
28SS	1	#A3C4BC	        14-4807 TCX	SS28_Forecast.pdf
```

---

### 📄 wgsn 탭

newsletter 탭과 **동일한 구조** 사용.

| 컬럼 | 형식 | 예시 |
|---|---|---|
| `season` | 텍스트 | `27FW` |
| `no` | 숫자 | `1` |
| `hex` | `#RRGGBB` | `#32575D` |
| `pantone` | 텍스트 | `19-4517 TCX` |
| `source_file` | 텍스트 | `WGSN_AW27.pdf` |

---

### 📄 peclers 탭

| 컬럼 | 형식 | 예시 | 설명 |
|---|---|---|---|
| `season` | 텍스트 | `27FW` | 시즌 코드 |
| `Theme` | 텍스트 | `01_Sensitive` | Peclers 테마명 |
| `Peclers_Color_Name_EN` | 텍스트 | `SEPIA` | 컬러명 (영문) |
| `Peclers_Color_Name_FR` | 텍스트 | `SÉPIA` | 컬러명 (불어, 없으면 빈칸) |
| `Pantone_TPG` | 텍스트 | `16-1510 TPG` | Pantone TPG 코드 |
| `Pantone_C` | 텍스트 | `2475 C` | Pantone C 코드 (없으면 빈칸) |
| `Coloro` | 텍스트 | `015-52-06` | Coloro 코드 (없으면 빈칸) |
| `Hex` | `#RRGGBB` | `#a28780` | 컬러 HEX 코드 |
| `Confidence` | `High / Medium / Low` | `High` | 신뢰도 |

```
season	Theme	        Peclers_Color_Name_EN	Hex	    Confidence
27FW	01_Sensitive	SEPIA	                #a28780	High
28SS	01_Sensitive	IVORY SAND	        #f0ede6	Medium
```

---

### 📄 market 탭

| 컬럼 | 형식 | 예시 | 설명 |
|---|---|---|---|
| `season` | 텍스트 | `27FW` | 시즌 코드 |
| `country` | `GL / KR / CN` | `GL` | 마켓 구분 — 자동으로 GL·KR·CN 분류 |
| `brand_group` | 텍스트 | `애슬레저` | 브랜드 그룹 |
| `brand` | 텍스트 | `ALO YOGA` | 브랜드명 |
| `gender` | 텍스트 | `Women` | 성별 |
| `category` | 텍스트 | `outerwear` | 카테고리 |
| `subcategory` | 텍스트 | `—` | 서브카테고리 |
| `fabric` | 텍스트 | | 소재 |
| `fabric_group` | 텍스트 | | 소재 그룹 |
| `product_name` | 텍스트 | `Gold Rush Puffer` | 상품명 |
| `color_names` | 텍스트 | `Black \| White` | 컬러명 (`\|` 구분) |
| `hex_codes` | `#RRGGBB \| #RRGGBB` | `#111111 \| #928E8B` | HEX 코드 (`\|` 구분, 멀티컬러 가능) |
| `color_count` | 숫자 | `2` | 컬러 수 |
| `pct_in_image` | 숫자 | | 이미지 내 비중 |

> ⚠️ `hex_codes` 컬럼에 컬러가 여러 개면 `|` 로 구분.  
> 예: `#111111 | #928E8B`

```
season	country	brand	        hex_codes
27FW	GL	ALO YOGA	#111111 | #928E8B
27FW	KR	MLB	        #1A1A1A
28SS	GL	LULULEMON	#D4C5B0
```

---

### 📄 mlbSales 탭

| 컬럼 | 형식 | 예시 | 설명 |
|---|---|---|---|
| `season` | 텍스트 | `27FW` | 시즌 코드 |
| `ITEM_GROUP` | 텍스트 | `다운` | 아이템 대분류 |
| `ITEM_NM` | 텍스트 | `다운점퍼` | 아이템명 |
| `PRDT_CD` | 텍스트 | `M25F3ADJB2656` | 상품 코드 |
| `PRDT_NM` | 텍스트 | `커브패딩` | 상품명 |
| `COLOR_CD` | 텍스트 | `50BKS` | MLB 컬러 코드 |
| `COLOR_CD_3` | 텍스트 | `BKS` | 3자리 컬러 코드 |
| `COLOR_NM` | 텍스트 | `BLACK` | 컬러명 |
| `COLOR_SYS` | 텍스트 | `실물칩` | 컬러 시스템 |
| `COLOR_SYS_CODE` | 텍스트 | `19-3911` | Pantone 코드 — 트렌드와 매칭 기준 |
| `SALE_AMT` | 숫자 | `2636396952` | 판매 금액 |
| `AC_SALE_AMT` | 숫자 | `2636396952` | 누적 판매 금액 |
| `AC_SALE_QTY` | 숫자 | `10351` | 누적 판매 수량 |
| `AC_ORD_QTY` | 숫자 | `25056` | 누적 발주 수량 |
| `AC_STOR_QTY` | 숫자 | `25113` | 누적 입고 수량 |
| `STOCK_QTY` | 숫자 | `14596` | 재고 수량 |
| `SALE_RT` | 소수 | `0.41` | 판매율 (0~1) |

---

## 새 시즌 데이터 추가하는 법

28SS 데이터가 생겼을 때 할 일은 딱 하나입니다.

```
① 각 탭(newsletter / wgsn / peclers / market / mlbSales)에
   season 컬럼에 "28SS" 라고 적은 행들을 추가

② 앱에서 사이드바 → 28SS → LOAD DATA 클릭
```

**URL도, 코드도 건드릴 필요 없음.**

---

## 파일 구조

```
FnF_Color_AI_v2.html    ← 앱 전체 (HTML + CSS + JS 단일 파일)
README.md               ← 이 문서
```

---

## 담당

F&F Consumer Strategy Team  
