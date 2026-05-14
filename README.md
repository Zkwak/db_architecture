# db_architecture

## 설계 전제
### - 데이터 소스: bloomberg (BDH, BDP 등) -> OUTPUT: CSV
### - 실무진 간 공유 및 직관성을 우선순위에 두기 위하여 SQLite 등 DB 미사용 -> Excel csv 사용
### - Python으로 정합성 및 자동화 처리하고, 실무진에 csv로 보고
### - 최초 history_raw를 curated 파일로 backfill 실행한 이후에는 매일 특정 시간에 데이터를 수집 & 기존 DB에 적재하는 방식으로 관리

## 디렉토리 구조 (IMPORTANT)
#### 1. raw
##### - history_raw.csv
##### - daily_raw_yyyymmdd.csv
#### 2. curated
##### - history_curated.csv
##### - curated_yyyymmdd.csv
##### - curated_log_yyyymmdd.csv
#### 3. scripts
##### - backfill.py (*) 과거 1회용
##### - daily_update.py (*) 매일 특성 시간 실행


## 전체 아키텍처 개요
### 1. raw CSV Layer
####- Bloomberg Excel Function에서 raw CSV Layer 추출
####- 수정 금지
### 2. Cleansing & Validation Layer (Python)
####- raw CSV Layer를 한국 영업일 기준 재정렬, 환율 이상치 처리, 이상치 로그 생성
### 3. Curated CSV Layer
#### - 분석, 성과, 리포트의 기준 데이터
#### - 1일 1행
#### - raw csv를 직접 참조하지 않음
### 4. 이상치 로그
#### - 이상치 처리에 대한 기록을 별도로 관리

## Directory 구조
