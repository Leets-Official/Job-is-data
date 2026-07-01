# Job.is — 채용공고 데이터 & 추출 파이프라인

Job.is(AI 커리어 큐레이션 서비스)의 **IT/개발 채용공고 데이터셋**과 그것을 생산하는 **수집 파이프라인**.
원티드(공고 본체) + 잡코리아(기업 메타 보강) 아키텍처.

## 구성 — 두 패키지

```
.
├── database/     ← 백엔드용: 바로 적재 가능한 PostgreSQL 데이터셋
│   ├── schema.sql · load.py · data/*.jsonl
│   └── README.md · DATA_DICTIONARY.md
└── extractor/    ← 데이터팀용: 수집/추출 파이프라인 (Python stdlib only, 무설치)
    ├── scripts/  (wanted · jobkorea_company · build_pool · shard_extract · merge_shards · skill_enrich)
    └── README.md · EXTRACTION_GUIDE.md · schema.sql
```

| 패키지 | 대상 | 하는 일 | 시작점 |
|---|---|---|---|
| **database/** | 백엔드팀 | 데이터를 Postgres에 적재해 서비스에 사용 | `database/README.md` |
| **extractor/** | 수집/데이터팀 | 파이프라인을 돌려 데이터를 생산·갱신 | `extractor/README.md` |

## 데이터 스냅샷

| | |
|---|---|
| 공고 / 기업 | **1,000 / 618** |
| 직군 | 개발/IT 100% (MVP 한정) |
| 커버리지 | JD·썸네일 100% · skill 95% · geo 99.7% |
| 기업 보강 | enriched 40% (나머지 원티드 업종 fallback) |

## 빠른 시작

```sh
# A. DB 적재 (백엔드)
cd database && pip install -r requirements.txt
export DATABASE_URL="postgresql://user:pass@host:5432/jobis"
python load.py --dir data --schema schema.sql --init

# B. 데이터 재생산 (수집팀, 무설치)
cd extractor/scripts && python3 run_pipeline.py --target 30 --out ./out
```

세부 내용은 각 패키지의 README / DATA_DICTIONARY / EXTRACTION_GUIDE 참고.
