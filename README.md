# Git Flow 실습 과제 가이드

## 1. 과제명

**Git Flow를 사용해 간단한 할 일 목록 배포하기**

## 2. 과제 목적

이 과제의 핵심은 웹 개발이 아니라 다음 Git Flow 과정을 직접 경험하는 것입니다.

```text
main → develop → feature → develop → release → main
                                      ↓
                                   develop

main → hotfix → main
          ↓
       develop
```

간단한 `index.html` 파일을 수정하면서 다음 브랜치를 모두 사용합니다.

- `main`: 배포 가능한 최종 버전
- `develop`: 개발 내용 통합
- `feature/*`: 기능 개발
- `release/*`: 출시 준비
- `hotfix/*`: 배포 후 긴급 수정

> 별도의 `git-flow` 확장 프로그램은 사용하지 않아도 됩니다. 기본 `git` 명령어만 사용합니다.

---

## 3. 준비물

- Git
- GitHub 계정
- 텍스트 편집기(VS Code, 메모장 등)
- 담당자가 제공한 과제 파일(`README.md`, `index.html`)
- 본인의 GitHub 계정에 생성할 개인 공개(Public) 저장소

Node.js, Python, 데이터베이스, 웹 서버는 필요하지 않습니다.

> 학생마다 개인 저장소를 사용하므로 GitHub Pages 설정도 각자의 저장소에서 한 번 수행해야 합니다. 설정이 끝난 뒤에는 `main` 브랜치에 push할 때마다 배포 페이지가 갱신됩니다.

---

## 4. 제출 결과물

다음 항목을 제출합니다.

1. GitHub 저장소 주소
2. GitHub Pages 배포 주소
3. `git log --graph` 실행 결과 화면
4. `v1.0.0`, `v1.0.1` 태그가 보이는 화면

---

## 5. 과제 파일 준비 및 개인 저장소 만들기

### 5.1 제공받은 파일 준비하기

담당자가 제공한 ZIP 파일의 압축을 풀거나, 전달받은 파일을 하나의 새 폴더에 저장합니다.

예시 폴더 구조:

```text
todo-git-flow/
├── README.md
└── index.html
```

터미널에서 해당 폴더로 이동합니다.

```bash
cd <과제-폴더-경로>
```

현재 폴더에 다음 두 파일이 있는지 확인합니다.

```text
README.md
index.html
```

> ZIP 파일 안에 상위 폴더가 포함되어 있다면 압축을 푼 뒤 `README.md`와 `index.html`이 들어 있는 폴더에서 명령어를 실행하세요.

### 5.2 GitHub에 개인 저장소 만들기

본인의 GitHub 계정에서 새로운 **Public 저장소**를 만듭니다.

권장 저장소 이름:

```text
todo-git-flow
```

저장소를 만들 때 다음 항목은 추가하지 마세요.

- README
- `.gitignore`
- License

로컬에 이미 과제 파일이 있으므로 GitHub 저장소는 **비어 있는 상태**로 만들어야 합니다.

### 5.3 로컬 Git 저장소 초기화하기

과제 파일이 있는 폴더에서 다음 명령어를 실행합니다.

```bash
git init -b main
git add README.md index.html
git commit -m "chore: Git Flow 실습 시작"
```

현재 브랜치를 확인합니다.

```bash
git branch --show-current
```

결과가 `main`이어야 합니다.

### 5.4 개인 GitHub 저장소 연결하기

본인이 만든 개인 저장소를 `origin`으로 연결합니다.

```bash
git remote add origin https://github.com/본인아이디/todo-git-flow.git
```

연결 결과를 확인합니다.

```bash
git remote -v
```

다음과 같이 본인의 저장소 주소가 표시되어야 합니다.

```text
origin  https://github.com/본인아이디/todo-git-flow.git (fetch)
origin  https://github.com/본인아이디/todo-git-flow.git (push)
```

### 5.5 개인 저장소에 `main` 올리기

```bash
git push -u origin main
```

GitHub에서 본인의 개인 저장소를 열어 `README.md`와 `index.html`이 정상적으로 올라갔는지 확인합니다.

---

## 6. `develop` 브랜치 만들기

`main`을 기준으로 개발 통합 브랜치를 만듭니다.

```bash
git switch main
git switch -c develop
git push -u origin develop
```

---

## 7. `feature` 브랜치에서 기능 추가하기

`develop`에서 기능 브랜치를 만듭니다.

```bash
git switch develop
git switch -c feature/todo-list
```

제공된 `index.html`을 열고 다음 주석을 찾습니다.

```html
<!-- feature/todo-list 브랜치에서 여기에 할 일 목록을 추가하세요. -->
```

주석을 다음 목록으로 교체합니다. hotfix 실습을 위해 두 번째 항목의 `과재`라는 오타는 지금 고치지 마세요.

```html
<ul>
  <li>Git 강의 복습하기</li>
  <li>과재 제출하기</li>
  <li><del>Git 설치하기</del> — 완료</li>
</ul>
```

변경 내용을 확인하고 커밋합니다.

```bash
git status
git add index.html
git commit -m "feat: 할 일 목록 기능 추가"
```

기능 브랜치를 원격 저장소에도 기록합니다.

```bash
git push -u origin feature/todo-list
```

`develop`에 병합합니다.

```bash
git switch develop
git merge --no-ff feature/todo-list -m "merge: 할 일 목록 기능 통합"
git push origin develop
```

병합을 확인한 뒤 로컬 기능 브랜치를 삭제합니다.

```bash
git branch -d feature/todo-list
```

> 채점 전에 브랜치 사용 기록을 확인할 수 있도록 원격 기능 브랜치는 남겨 두거나, 삭제 전 브랜치 화면을 캡처하세요.

---

## 8. `release` 브랜치에서 출시 준비하기

`develop`에서 출시 브랜치를 만듭니다.

```bash
git switch develop
git switch -c release/v1.0.0
```

`index.html`의 footer에서 `개발 중`을 `1.0.0`으로 변경합니다. 이 단계에서는 `과재` 오타를 고치지 마세요.

```html
<footer>
  버전: 1.0.0
</footer>
```

수정 내용을 커밋하고 원격 저장소에 올립니다.

```bash
git add index.html
git commit -m "chore: v1.0.0 출시 준비"
git push -u origin release/v1.0.0
```

### `main`에 병합하고 태그 붙이기

```bash
git switch main
git merge --no-ff release/v1.0.0 -m "release: v1.0.0"
git tag -a v1.0.0 -m "첫 번째 배포"
```

### `develop`에도 병합하기

```bash
git switch develop
git merge --no-ff release/v1.0.0 -m "merge: v1.0.0 변경 사항 반영"
```

결과를 올립니다.

```bash
git push origin main develop
git push origin v1.0.0
git branch -d release/v1.0.0
```

이 시점의 `main`이 최초 배포 버전입니다.

---

## 9. `hotfix` 브랜치에서 긴급 수정하기

배포 후 오타가 발견되었다고 가정합니다. `main`에서 hotfix 브랜치를 만듭니다.

```bash
git switch main
git switch -c hotfix/v1.0.1
```

`index.html`에서 `과재 제출하기`를 `과제 제출하기`로 수정하고, footer의 버전을 `1.0.0`에서 `1.0.1`로 변경합니다.

```bash
git add index.html
git commit -m "fix: 할 일 목록 오타 수정"
git push -u origin hotfix/v1.0.1
```

### `main`에 병합하고 새 태그 붙이기

```bash
git switch main
git merge --no-ff hotfix/v1.0.1 -m "hotfix: v1.0.1"
git tag -a v1.0.1 -m "오타 수정 배포"
```

### `develop`에도 수정 내용 반영하기

```bash
git switch develop
git merge --no-ff hotfix/v1.0.1 -m "merge: v1.0.1 긴급 수정 반영"
```

최종 결과를 올립니다.

```bash
git push origin main develop
git push origin v1.0.1
git branch -d hotfix/v1.0.1
```

---

## 10. GitHub Pages로 배포하기

GitHub 저장소에서 다음 메뉴로 이동합니다.

1. **Settings**
2. **Pages**
3. **Build and deployment**의 Source를 **Deploy from a branch**로 선택
4. Branch를 `main`, 폴더를 `/ (root)`로 선택
5. **Save** 클릭

잠시 후 다음과 비슷한 주소가 만들어집니다.

```text
https://사용자이름.github.io/저장소이름/
```

이후에는 `main`에 변경 사항을 push하면 배포 페이지도 갱신됩니다.

> 이 과제에서는 학생이 만든 개인 저장소를 사용하므로 각자 자신의 저장소에서 Pages를 설정해야 합니다.

---

## 11. 최종 확인

### 브랜치 확인

```bash
git branch -a
```

### 태그 확인

```bash
git tag
```

다음 두 태그가 있어야 합니다.

```text
v1.0.0
v1.0.1
```

### 커밋 그래프 확인

```bash
git log --graph --oneline --decorate --all
```

### 작업 상태 확인

```bash
git status
```

다음 메시지가 나오면 정상입니다.

```text
nothing to commit, working tree clean
```

---

## 12. 체크리스트

- [ ] 제공받은 `README.md`와 `index.html`을 하나의 폴더에 준비했다.
- [ ] 본인의 GitHub 계정에 비어 있는 Public 저장소를 만들었다.
- [ ] 로컬 저장소를 `main` 브랜치로 초기화하고 시작 커밋을 만들었다.
- [ ] 개인 저장소를 `origin`으로 연결했다.
- [ ] 개인 저장소에 `main`을 push했다.
- [ ] `main`에서 시작했다.
- [ ] `develop` 브랜치를 만들었다.
- [ ] `feature/todo-list`에서 기능을 추가했다.
- [ ] feature를 `develop`에 `--no-ff`로 병합했다.
- [ ] `release/v1.0.0`에서 출시 준비 작업을 했다.
- [ ] release를 `main`과 `develop`에 병합했다.
- [ ] `v1.0.0` 태그를 만들었다.
- [ ] `hotfix/v1.0.1`에서 긴급 수정을 했다.
- [ ] hotfix를 `main`과 `develop`에 병합했다.
- [ ] `v1.0.1` 태그를 만들었다.
- [ ] 모든 커밋과 태그를 GitHub에 push했다.
- [ ] GitHub Pages 배포 주소가 정상적으로 열린다.
- [ ] 저장소 주소, 배포 주소, 커밋 그래프를 제출했다.

---

## 13. 주의 사항

- 작업하기 전에 현재 브랜치를 항상 확인하세요.

  ```bash
  git branch --show-current
  ```

- `feature`와 `release`는 `develop`에서 만듭니다.
- `hotfix`는 배포 버전인 `main`에서 만듭니다.
- release와 hotfix 결과는 `main`뿐 아니라 `develop`에도 반영해야 합니다.
- `git merge --no-ff`를 사용해야 병합 기록이 명확하게 남습니다.
- 강제 push 명령인 `git push --force`는 사용하지 마세요.
- GitHub 웹사이트에서 파일을 직접 수정하지 말고, 로컬에서 수정·커밋한 후 push하세요.
- `origin`은 반드시 본인의 개인 저장소를 가리켜야 합니다.
- GitHub에서 개인 저장소를 만들 때 README, `.gitignore`, License를 미리 추가하지 마세요.

---

## 14. 한눈에 보는 전체 흐름

```bash
# 제공받은 README.md와 index.html이 있는 폴더로 이동
cd <과제-폴더-경로>

# 로컬 저장소 초기화 및 개인 저장소 연결
git init -b main
git add README.md index.html
git commit -m "chore: Git Flow 실습 시작"
git remote add origin https://github.com/본인아이디/todo-git-flow.git
git push -u origin main

# develop
git switch main
git switch -c develop
git push -u origin develop

# feature
git switch -c feature/todo-list
# index.html 수정
git add index.html
git commit -m "feat: 할 일 목록 기능 추가"
git push -u origin feature/todo-list
git switch develop
git merge --no-ff feature/todo-list -m "merge: 할 일 목록 기능 통합"

# release
git switch -c release/v1.0.0
# 버전 정보 수정
git add index.html
git commit -m "chore: v1.0.0 출시 준비"
git push -u origin release/v1.0.0
git switch main
git merge --no-ff release/v1.0.0 -m "release: v1.0.0"
git tag -a v1.0.0 -m "첫 번째 배포"
git switch develop
git merge --no-ff release/v1.0.0 -m "merge: v1.0.0 변경 사항 반영"

# hotfix
git switch main
git switch -c hotfix/v1.0.1
# 오타와 버전 정보 수정
git add index.html
git commit -m "fix: 할 일 목록 오타 수정"
git push -u origin hotfix/v1.0.1
git switch main
git merge --no-ff hotfix/v1.0.1 -m "hotfix: v1.0.1"
git tag -a v1.0.1 -m "오타 수정 배포"
git switch develop
git merge --no-ff hotfix/v1.0.1 -m "merge: v1.0.1 긴급 수정 반영"

# 최종 push
git push origin main develop
git push origin --tags

# 확인
git log --graph --oneline --decorate --all
```
