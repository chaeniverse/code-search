# code-search

키워드·방법·논문에 맞는 공개 코드를 찾고, 믿을 만한지 확인하고, 원하면 받아서 정리하는 Claude Code 스킬(플러그인).

A Claude Code skill and subagent for finding and vetting public code — GitHub repositories, official paper implementations, Hugging Face models/datasets, and PyPI/CRAN packages — for a keyword, method, or paper list, with optional read-only shallow clones. Instructions are written in Korean.

## 하는 일

키워드(또는 논문 제목·arXiv ID 목록, 패키지 이름)를 주면 포함 조건 한 줄 → 여러 경로 검색 → 중복·fork 정리 → 신뢰도 확인 → Tier A/B → (원하면) shallow clone → 코드 카드로 정리한다.

- 검색 경로: GitHub(`gh search repos`·`gh search code`) · 논문 구현(HF Papers · arXiv · Zenodo) · Hugging Face Hub · PyPI · CRAN · Bioconductor
- 신뢰도 신호: 공식 여부, 마지막 커밋·릴리스·archived, 라이선스, 환경 파일·가중치 공개, tests·CI. 스타 수는 다른 신호를 대신하지 않는다
- 갈래가 셋 이상이면 `code-scout` 서브에이전트를 갈래마다 병렬로 띄운다
- 하지 않는 것: 받은 코드 실행, 라이선스 없는 코드 복사, 이슈·스타·fork 같은 외부 동작, 미발표 연구 세부를 검색어에 넣기

규칙 전체는 [skills/code-search/SKILL.md](skills/code-search/SKILL.md), 서브에이전트는 [agents/code-scout.md](agents/code-scout.md).

자매 스킬 [lit-search](https://github.com/chaeniverse/lit-search) 로 논문을 찾고, 그 결과(노트·JSON)를 이 스킬에 목록으로 넘겨 공식 구현을 찾을 수 있다.

## 설치

플러그인으로:

```
/plugin marketplace add chaeniverse/code-search
/plugin install code-search@code-search
```

또는 스킬 폴더와 에이전트만 연결:

```bash
git clone https://github.com/chaeniverse/code-search.git
ln -s "$PWD/code-search/skills/code-search" ~/.claude/skills/code-search
mkdir -p ~/.claude/agents && ln -s "$PWD/code-search/agents/code-scout.md" ~/.claude/agents/code-scout.md
```

## 설정

`~/.config/code-search/config.json` 에 둔다. 저장소에는 넣지 않는다. 형식은 [config.example.json](config.example.json).

| 키 | 뜻 |
|---|---|
| `code_root` | clone 저장 루트. 실행마다 `YYYY-MM-DD <주제>` 하위 폴더를 만든다 |
| `default_languages` | 언어 기본값. 비우면 제한하지 않는다 |
| `json_export_dir` | 검색 결과 JSON 을 쓸 폴더(선택). 형식은 [docs/json-schema.md](docs/json-schema.md) |

설정 파일이 없으면 처음 clone 할 때 저장 위치를 묻고, 원하면 파일을 만들어 준다.
토큰·아이디·이메일은 설정에 넣지 않는다.

필요한 것: Claude Code, 로그인된 [GitHub CLI](https://cli.github.com/)(`gh auth login`, 코드 검색에 인증이 필요하다), `curl`, `git`.

## 기여

이슈 → 브랜치 → PR 순서로 한다. 브랜치·커밋·PR 규칙은 [docs/git-workflow.md](docs/git-workflow.md).

## 라이선스

MIT
