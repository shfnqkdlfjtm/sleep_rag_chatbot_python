sleep_counselor_chatbot/
├─ pyproject.toml                # (권장) poetry/pip-tools 사용 시
├─ setup.cfg                     # flake8/black/isort/pytest 공통 설정
├─ README.md
├─ .env.example                  # 필요한 키 목록 (실키는 .env에)
├─ .gitignore
├─ scripts/                      # 개발/배포 스크립트
│  ├─ run_app.sh
│  ├─ build_embeddings.py        # 일괄 색인 파이프라인
│  └─ eval_rag_offline.py        # 오프라인 평가(A/B)
├─ data/                         # (옵션) 샘플 데이터/스키마
│  └─ samples/
├─ docs/                         # 문서(아키텍처, API, 프롬프트 표준)
│  └─ architecture.md
├─ app.py                        # Streamlit 진입(최소화)
├─ run.py                        # 편의 실행(로컬 전용)
├─ src/
│  └─ sleepbot/                  # ← 최상위 패키지(이름 통일 권장)
│     ├─ ui/                     # UI 레이어(순수 프레젠테이션)
│     │  ├─ pages/               # (여러 페이지 사용 시)
│     │  │  ├─ home.py
│     │  │  └─ chat.py
│     │  ├─ components/
│     │  │  ├─ sidebar.py
│     │  │  ├─ message_display.py
│     │  │  ├─ chat_input.py
│     │  │  └─ styles.py
│     │  └─ app_shell.py         # 레이아웃/라우팅/토스트 등
│     ├─ application/            # 앱 서비스(유즈케이스) ─ UI↔도메인 연결
│     │  ├─ controller.py        # UI 이벤트→서비스 호출 오케스트레이션
│     │  └─ state.py             # 세션 상태/옵션(Top-k, 임계)
│     ├─ domain/                 # 도메인 모델·정책(프레임워크 무관)
│     │  ├─ models/
│     │  │  └─ document.py       # id,title,lang,page,section,url,text,score…
│     │  ├─ prompts/
│     │  │  ├─ personas.py
│     │  │  └─ cbti_weeks.py     # ✅ utils↔core의 중복 제거→여기로 통합
│     │  └─ services/
│     │     └─ chatbot.py        # 대화 정책(요지/근거/주의) 규약, I/O 포맷
│     ├─ rag/                    # RAG 파이프라인(검색·임베딩)
│     │  ├─ pipeline.py          # 색인/검색/재배치/컨텍스트 조립 오케스트라
│     │  ├─ embedding.py         # 임베딩 클라이언트 어댑터
│     │  ├─ retrieval.py         # 검색(Top-k, threshold, MMR)
│     │  ├─ ranking.py           # (옵션) 재순위/스코어링
│     │  └─ schemas.py           # 검색 결과/근거 스키마
│     ├─ infra/                  # 인프라 어댑터(외부 의존)
│     │  ├─ vectorstores/
│     │  │  ├─ qdrant_store.py
│     │  │  └─ pinecone_store.py
│     │  ├─ llm/
│     │  │  └─ openai_client.py
│     │  ├─ audio/
│     │  │  ├─ stt_client.py
│     │  │  └─ tts_client.py
│     │  └─ config.py            # 환경 로딩(typed), 로거/tracing 설정
│     ├─ utils/
│     │  ├─ error.py             # 예외/사용자 메시지 변환
│     │  ├─ text.py              # 전처리/언어감지/토큰카운트
│     │  └─ timing.py            # 지연/비용 측정 헬퍼
│     └─ __init__.py
└─ tests/
   ├─ unit/
   │  ├─ test_retrieval.py
   │  ├─ test_embedding.py
   │  └─ test_cbti_prompts.py
   └─ e2e/
      └─ test_chat_flow.py
