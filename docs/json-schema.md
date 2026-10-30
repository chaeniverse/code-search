# JSON 내보내기 형식

검색 결과를 정적 사이트(예: Vercel에 올린 Next.js 뷰어)에서 읽기 위한 형식이다. 쓰지 않아도 스킬은 그대로 동작한다. 틀은 lit-search 의 JSON 과 같고, `items[]` 의 필드만 코드에 맞게 다르다. 그래서 같은 뷰어로 두 결과를 함께 보여 줄 수 있다.

한 번 실행이 한 파일이다: `<json_export_dir>/<YYYY-MM-DD>.json`. 같은 날 두 번 돌리면 `<YYYY-MM-DD>-2.json`.

```json
{
  "kind": "code",
  "date": "2026-10-30",
  "title": "2026-10-30 생성모델 평가 지표",
  "query": "생성모델 평가 지표(FID·precision/recall 계열)를 … 계산하는 Python 구현, 가져다 쓰기용",
  "meta": [
    { "label": "훑어본 범위", "value": "저장소 약 180개 → 신뢰도 확인 22개" },
    { "label": "수록", "value": "Tier A 3개 / Tier B 6개" }
  ],
  "overview": ["이번 검색 요약 — 줄마다 한 문단"],
  "sections": [
    {
      "category": "Tier A",
      "items": [
        {
          "no": "1",
          "name": "owner/repo",
          "url": "https://github.com/owner/repo",
          "type": "저장소",
          "language": "Python · PyTorch",
          "license": "Apache-2.0",
          "last_commit": "2026-09-12",
          "status": "스타 3.8k · 최근 릴리스 v0.3.1 (2026-08) · CI 있음",
          "official": "공식 — arXiv 초록의 코드 링크",
          "paper": "arXiv:2403.09669",
          "summary": "한국어 2~4문장 요약",
          "usage": "pip install … / src/metrics/fid.py 의 compute_fid()",
          "reproducibility": "requirements.txt, 가중치 자동 다운로드, GPU 필요 없음",
          "relevance": "내 연구와의 연결 한두 문장",
          "questions": ["확인할 점 1"],
          "notice": "",
          "clone": "2026-10-30 생성모델 평가 지표/owner__repo @ 1a2b3c4"
        }
      ]
    }
  ],
  "dropped_title": "포함 조건·신뢰도로 뺀 것",
  "dropped": [{ "name": "…", "url": "…", "reason": "라이선스 없음, 가져다 쓰기 용도" }],
  "memo": ["검색한 갈래와 검색어, 확보 못 한 것과 사유"]
}
```

| 필드 | 뜻 |
|---|---|
| `kind` | `code`. lit-search 결과와 한 뷰어에서 구분할 때 쓴다(lit-search 쪽은 없으면 `paper` 로 본다) |
| `date` · `title` · `query` | 실행일, 제목, 포함 조건 한 줄 |
| `meta` | 보고 머리말을 `{label, value}` 로 |
| `overview` | 전체 요약 — 줄마다 한 문단 |
| `sections[]` | `Tier A` · `Tier B` · `그 외 수집 목록` |
| `sections[].items[]` | 코드 카드. 스킬 5단계 항목과 같다. 없는 값은 빈 문자열 |
| `dropped_title` · `dropped[]` | 뺀 것 기록 |
| `memo[]` | 검색 메모 |

- `seen.json`(선택): 다룬 저장소 식별자(`github.com/owner/repo` 등 정규화한 값) 목록. 반복 실행할 때 중복을 막는다.
- `clone` 에는 `code_root` 기준 상대 경로와 커밋 SHA 만 넣는다. 절대 경로는 넣지 않는다. 받은 코드 자체는 저장소·사이트에 올리지 않는다.
- 파생 앱이 자기 필드를 더 붙여도 된다. 뷰어는 모르는 필드를 무시하게 만든다.
- 미발표 연구와 연결 짓는 메모(`relevance`·`questions`)가 들어가면 사이트를 비공개로 두거나 인증을 건다.
