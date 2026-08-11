# 🧩 Build Visual CMS

기존 웹사이트를 분석해 **실제 화면과 연결된 맞춤형 CMS**를 설계하고 구현하도록 돕는 Agent Skill입니다.

- 실제 공개 컴포넌트를 미리보기에 재사용
- 편집 필드와 미리보기 요소를 양방향으로 선택·이동
- 편집기와 미리보기의 스크롤을 독립적으로 유지
- 미리보기와 구분되는 밝고 중립적인 편집기 배경
- 텍스트, 이미지와 항상 제공되는 파비콘·프라이머리 색상·글씨체 공통 설정
- 실제 리스트·그리드·비대칭 배치를 편집기에도 반영
- 왼쪽 편집기 하단의 `반영하기` 버튼 하나로 변경 적용
- **Codex와 Claude Code에서 공통 사용 가능**

## 🚀 설치

### 한 번에 설치

```bash
npx skills add SheepJH/build-visual-cms@build-visual-cms -g -y
```

설치한 뒤 Codex 또는 Claude Code를 새로 시작하세요.

<details>
<summary><strong>Codex에서 직접 설치하기</strong></summary>

Codex에 다음과 같이 요청합니다.

```text
$skill-installer로 https://github.com/SheepJH/build-visual-cms/tree/main/skills/build-visual-cms 스킬을 설치해줘.
```

설치 후 다음 대화부터 사용할 수 있습니다.

</details>

<details>
<summary><strong>Claude Code에 수동 설치하기</strong></summary>

모든 프로젝트에서 사용하려면:

```bash
git clone --depth 1 https://github.com/SheepJH/build-visual-cms.git /tmp/build-visual-cms
mkdir -p ~/.claude/skills
cp -R /tmp/build-visual-cms/skills/build-visual-cms ~/.claude/skills/build-visual-cms
```

특정 프로젝트에서만 사용하려면 스킬 폴더를 해당 프로젝트의 `.claude/skills/build-visual-cms`에 복사하세요.

</details>

## 🛠️ 사용

작업할 웹 프로젝트를 연 뒤 이렇게 요청하세요.

```text
$build-visual-cms를 사용해서 이 프로젝트를 분석하고,
기존 디자인을 유지하는 시각적 CMS를 구현해줘.
```

구현 전에 설계만 확인할 수도 있습니다.

```text
$build-visual-cms를 사용해서 CMS 도입안을 먼저 작성해줘. 파일은 수정하지 마.
```

## 📌 주요 규칙

<details>
<summary><strong>미리보기와 선택 동기화</strong></summary>

- 미리보기는 실제 공개 컴포넌트를 사용합니다.
- 편집 필드를 선택하면 대응하는 미리보기 요소로 이동해 강조합니다.
- 미리보기 요소를 선택하면 정확한 편집 필드를 엽니다.
- 선택한 쪽은 움직이지 않고 반대쪽 영역만 필요한 위치로 이동합니다.
- 편집기와 미리보기는 각각 독립적으로 스크롤합니다.

</details>

<details>
<summary><strong>레이아웃과 편집 환경</strong></summary>

- 실시간 미리보기를 중심에 두고 편집 패널은 필요한 만큼만 사용합니다.
- 왼쪽은 편집기, 오른쪽은 실시간 미리보기로 명확하게 분리합니다.
- 편집기는 미리보기와 섞이지 않는 밝고 중립적이거나 명확히 대비되는 배경을 사용합니다.
- 공개 화면이 리스트이면 리스트, 그리드이면 열·순서·강조·span까지 보존합니다.
- 세로·가로 방향, 그룹, 계층을 편집기에서도 미리보기와 거의 동일하게 알아볼 수 있도록 표현합니다.
- 미리보기 높이가 잘리지 않도록 영역 내부 스크롤로 모든 콘텐츠에 접근하게 합니다.
- 전체 화면은 최대한 깔끔하고 직관적이며 간결하게 구성합니다.

</details>

<details>
<summary><strong>텍스트·이미지·공통 설정</strong></summary>

- 문구, 버튼, 링크처럼 작은 필드까지 선택하고 편집할 수 있게 합니다.
- 운영자가 바꿀 수 있는 콘텐츠·배경·반응형 이미지와 로고를 조사해 편집 대상으로 만듭니다.
- 파비콘이 없어도 공통 설정에 항상 추가·교체 필드를 제공합니다.
- 공통 설정에는 프라이머리 색상과 글씨체 변경 항목을 항상 제공합니다.
- 왼쪽 편집기 상단은 `공통 설정 / 페이지` 두 영역으로 단순하게 구분합니다.
- 공통 설정은 브랜드·스타일·헤더·푸터로 묶고 로고와 여러 페이지에서 공유하는 메뉴 이름·링크·순서를 함께 관리합니다.
- 특정 페이지에서만 사용하는 탭이나 메뉴는 공통 설정이 아니라 해당 페이지에 둡니다.
- 프라이머리 색상을 바꾸면 사이트 전체의 해당 색상 토큰이 일관되게 변경되도록 합니다.
- 글씨체를 바꾸면 사이트 전체의 해당 텍스트에 일관되게 적용하고 실제로 로드되는 안전한 폰트 선택지만 제공합니다.

</details>

<details>
<summary><strong>반복 콘텐츠와 드래그 앤 드롭</strong></summary>

- 전용 손잡이, 삽입 위치 표시, 드래그 미리보기와 취소 기능을 제공합니다.
- 긴 목록에서는 편집기 영역만 자동 스크롤합니다.
- 별도의 위·아래 화살표 버튼은 두지 않습니다.
- 드래그 손잡이에서 키보드 이동을 지원하고 항목 ID로 선택 상태를 유지합니다.

</details>

<details>
<summary><strong>반영하기와 안전한 구현</strong></summary>

- 기존 프레임워크, 디자인 시스템, 인증과 저장 방식을 먼저 재사용합니다.
- 비밀키와 운영 설정을 일반 CMS 필드로 노출하지 않습니다.
- 상단의 초안 저장·발행 버튼과 되돌리기·앞으로 돌리기 버튼은 두지 않습니다.
- 왼쪽 편집기 맨 아래에 `반영하기` 버튼 하나만 두고 진행·완료·실패 상태를 간결하게 표시합니다.
- 승인 없이 배포하거나 운영 데이터를 마이그레이션하지 않습니다.
- 감사나 제안만 요청받으면 파일을 수정하지 않습니다.

</details>

## 📂 구조

```text
skills/build-visual-cms/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── cms-architecture.md
    ├── content-discovery.md
    ├── editor-ux.md
    └── validation-checklist.md
```

`SKILL.md`와 `references/`는 Codex와 Claude가 함께 사용하며, `agents/openai.yaml`은 Codex용 표시 정보입니다.

## 📝 커밋 규칙

Conventional Commits 형식과 한국어 설명을 사용합니다.

```text
feat: 편집기와 미리보기의 선택 동기화 추가
fix: 세부 문구가 상위 영역으로 선택되는 문제 수정
docs: Codex와 Claude 설치 방법 보완
```
