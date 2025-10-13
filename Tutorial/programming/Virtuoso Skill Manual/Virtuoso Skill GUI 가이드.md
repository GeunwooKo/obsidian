# Virtuoso SKILL GUI 프로그래밍 가이드

## 목차
- [[#GUI 기초]]
- [[#폼 생성]]
- [[#입력 필드]]
- [[#버튼과 이벤트]]
- [[#레이아웃 관리]]
- [[#고급 위젯]]
- [[#실전 예제]]

---

## GUI 기초

### 기본 대화상자

```skill
; 정보 메시지
procedure(showInfo(message)
    hiDisplayAppDBox(
        ?name 'infoBox
        ?dboxBanner "정보"
        ?dboxText message
        ?buttonLayout 'OKCancel
    )
)

; 예/아니오 확인
procedure(askYesNo(message)
    hiDisplayAppDBox(
        ?name 'confirmBox
        ?dboxBanner "확인"
        ?dboxText message
        ?buttonLayout 'YesNo
    )
)
```

### 간단한 입력 대화상자

```skill
procedure(inputDialog(prompt defaultValue)
    let((result)
        result = hiDisplayAppDBox(
            ?name 'inputBox
            ?dboxBanner "입력"
            ?dboxText prompt
            ?fields list(
                hiCreateStringField(
                    ?name 'input
                    ?prompt "값"
                    ?value defaultValue
                )
            )
            ?buttonLayout 'OKCancel
        )
        
        if(result then
            result->input->value
        else
            nil
        )
    )
)
```

---

## 폼 생성

### 기본 폼 구조

```skill
procedure(createBasicForm()
    let((form fields)
        fields = list(
            hiCreateStringField(
                ?name 'deviceName
                ?prompt "Device Name"
                ?value "M1"
            )
            hiCreateFloatField(
                ?name 'width
                ?prompt "Width (um)"
                ?value 1.0
            )
        )
        
        form = hiCreateAppForm(
            ?name 'myForm
            ?formTitle "Device Parameters"
            ?fields fields
            ?callback list("onApply()")
        )
        
        hiDisplayForm(form)
    )
)

procedure(onApply()
    let((form name width)
        form = hiGetCurrentForm()
        name = form->deviceName->value
        width = form->width->value
        printf("Device: %s, Width: %.2f\n" name width)
    )
)
```

---

## 입력 필드

### 문자열 입력

```skill
hiCreateStringField(
    ?name 'stringField
    ?prompt "문자열"
    ?value "기본값"
    ?editable t
)
```

### 숫자 입력

```skill
; 정수
hiCreateIntField(
    ?name 'intField
    ?prompt "정수"
    ?value 10
    ?range list(0 100)
)

; 실수
hiCreateFloatField(
    ?name 'floatField
    ?prompt "실수"
    ?value 3.14
    ?range list(0.0 10.0)
)
```

### 선택 필드

```skill
; 드롭다운
hiCreateCyclicField(
    ?name 'cyclic
    ?prompt "선택"
    ?choices list("A" "B" "C")
    ?value "A"
)

; 리스트박스
hiCreateListBoxField(
    ?name 'listbox
    ?prompt "항목"
    ?choices list("Item1" "Item2" "Item3")
    ?value "Item1"
)
```

### 체크박스

```skill
hiCreateBooleanButton(
    ?name 'checkbox
    ?buttonText "활성화"
    ?value t
)
```

---

## 버튼과 이벤트

### 버튼 생성

```skill
hiCreateButton(
    ?name 'myButton
    ?buttonText "실행"
    ?callback list("onButtonClick()")
)

procedure(onButtonClick()
    let((form)
        form = hiGetCurrentForm()
        printf("버튼 클릭!\n")
    )
)
```

---

## 고급 위젯

### 진행 표시줄

```skill
let((progressForm)
    procedure(showProgress(title)
        progressForm = hiCreateAppForm(
            ?name 'progressForm
            ?formTitle title
            ?fields list(
                hiCreateLabel(
                    ?name 'progressLabel
                    ?labelText "0%"
                )
            )
            ?buttonLayout 'None
        )
        hiDisplayForm(progressForm)
    )
    
    procedure(updateProgress(current total)
        let((percent text)
            percent = 100.0 * current / total
            text = sprintf(nil "%.1f%% (%d/%d)" 
                percent current total)
            progressForm->progressLabel->labelText = text
            hiRedraw(progressForm)
        )
    )
    
    procedure(hideProgress()
        hiDismissForm(progressForm)
    )
)
```

### 탭 인터페이스

```skill
procedure(createTabbedForm()
    let((form)
        form = hiCreateAppForm(
            ?name 'tabbedForm
            ?formTitle "설정"
            ?fields list(
                hiCreateTabField(
                    ?name 'tabs
                    ?tabs list(
                        list(
                            "기본"
                            hiCreateStringField(
                                ?name 'name
                                ?prompt "이름"
                            )
                        )
                        list(
                            "고급"
                            hiCreateIntField(
                                ?name 'level
                                ?prompt "레벨"
                            )
                        )
                    )
                )
            )
        )
        hiDisplayForm(form)
    )
)
```

---

## 실전 예제

### 예제 1: 트랜지스터 생성 GUI

```skill
procedure(transistorGUI()
    let((form)
        form = hiCreateAppForm(
            ?name 'transistorForm
            ?formTitle "트랜지스터 생성"
            
            ?fields list(
                ; 기본 정보
                hiCreateLabel(
                    ?name 'basicLabel
                    ?labelText "=== 기본 설정 ==="
                )
                hiCreateStringField(
                    ?name 'name
                    ?prompt "이름"
                    ?value "M1"
                )
                hiCreateCyclicField(
                    ?name 'type
                    ?prompt "타입"
                    ?choices list("NMOS" "PMOS")
                    ?value "NMOS"
                )
                
                ; 치수
                hiCreateLabel(
                    ?name 'sizeLabel
                    ?labelText "=== 치수 ==="
                )
                hiCreateFloatField(
                    ?name 'width
                    ?prompt "Width (um)"
                    ?value 1.0
                    ?range list(0.1 100.0)
                )
                hiCreateFloatField(
                    ?name 'length
                    ?prompt "Length (um)"
                    ?value 0.18
                    ?range list(0.1 10.0)
                )
                hiCreateIntField(
                    ?name 'fingers
                    ?prompt "Fingers"
                    ?value 1
                    ?range list(1 100)
                )
                
                ; 위치
                hiCreateLabel(
                    ?name 'posLabel
                    ?labelText "=== 위치 ==="
                )
                hiCreateFloatField(
                    ?name 'x
                    ?prompt "X"
                    ?value 0.0
                )
                hiCreateFloatField(
                    ?name 'y
                    ?prompt "Y"
                    ?value 0.0
                )
            )
            
            ?callback list("createTransistor()")
            ?buttonLayout 'OKCancelApply
        )
        
        hiDisplayForm(form)
    )
)

procedure(createTransistor()
    let((form cvId)
        form = hiGetCurrentForm()
        
        ; 셀뷰 열기
        cvId = geGetEditCellView()
        
        if(cvId then
            ; 트랜지스터 생성
            createParametricTransistor(
                cvId
                form->name->value
                ?width form->width->value
                ?length form->length->value
                ?fingers form->fingers->value
                ?type form->type->value
                ?x form->x->value
                ?y form->y->value
            )
            
            printf("트랜지스터 생성 완료\n")
        else
            warn("레이아웃을 먼저 여세요")
        )
    )
)
```

### 예제 2: DRC 체크 GUI

```skill
procedure(drcCheckGUI()
    let((form)
        form = hiCreateAppForm(
            ?name 'drcForm
            ?formTitle "DRC 체크"
            
            ?fields list(
                hiCreateLabel(
                    ?name 'layerLabel
                    ?labelText "레이어 선택"
                )
                hiCreateIntField(
                    ?name 'layerNum
                    ?prompt "Layer Number"
                    ?value 10
                )
                hiCreateStringField(
                    ?name 'purpose
                    ?prompt "Purpose"
                    ?value "drawing"
                )
                
                hiCreateLabel(
                    ?name 'ruleLabel
                    ?labelText "=== 룰 ==="
                )
                hiCreateFloatField(
                    ?name 'minWidth
                    ?prompt "Min Width (um)"
                    ?value 0.14
                )
                hiCreateFloatField(
                    ?name 'minSpacing
                    ?prompt "Min Spacing (um)"
                    ?value 0.14
                )
                
                hiCreateButton(
                    ?name 'runButton
                    ?buttonText "DRC 실행"
                    ?callback list("runDRC()")
                )
                
                hiCreateTextDisplayField(
                    ?name 'results
                    ?value ""
                    ?numRows 10
                )
            )
        )
        
        hiDisplayForm(form)
    )
)

procedure(runDRC()
    let((form cvId layer violations)
        form = hiGetCurrentForm()
        cvId = geGetEditCellView()
        
        when(cvId
            layer = list(
                form->layerNum->value
                form->purpose->value
            )
            
            ; DRC 실행
            violations = checkMinWidth(
                cvId layer form->minWidth->value
            )
            
            ; 결과 표시
            form->results->value = sprintf(nil
                "DRC 완료\n위반: %d개\n"
                length(violations)
            )
        )
    )
)
```

### 예제 3: 파일 브라우저

```skill
procedure(fileBrowserGUI()
    let((form)
        form = hiCreateAppForm(
            ?name 'browserForm
            ?formTitle "파일 브라우저"
            
            ?fields list(
                hiCreateStringField(
                    ?name 'path
                    ?prompt "경로"
                    ?value "./"
                )
                hiCreateButton(
                    ?name 'browseButton
                    ?buttonText "찾아보기"
                    ?callback list("browsePath()")
                )
                
                hiCreateListBoxField(
                    ?name 'fileList
                    ?prompt "파일"
                    ?choices list()
                )
                
                hiCreateButton(
                    ?name 'loadButton
                    ?buttonText "불러오기"
                    ?callback list("loadFile()")
                )
            )
        )
        
        hiDisplayForm(form)
        refreshFileList()
    )
)

procedure(refreshFileList()
    let((form path files)
        form = hiGetCurrentForm()
        path = form->path->value
        
        ; 파일 목록 가져오기
        files = getFileList(path)
        
        ; 리스트 업데이트
        form->fileList->choices = files
    )
)
```

### 예제 4: 설정 관리 GUI

```skill
let((configForm configData)
    configData = makeTable('configData nil)
    
    procedure(settingsGUI()
        ; 기본 설정 로드
        loadSettings()
        
        configForm = hiCreateAppForm(
            ?name 'settingsForm
            ?formTitle "설정"
            
            ?fields list(
                hiCreateLabel(
                    ?name 'generalLabel
                    ?labelText "=== 일반 ==="
                )
                hiCreateFloatField(
                    ?name 'gridSize
                    ?prompt "Grid Size"
                    ?value configData["gridSize"]
                )
                hiCreateBooleanButton(
                    ?name 'autoSave
                    ?buttonText "자동 저장"
                    ?value configData["autoSave"]
                )
                
                hiCreateLabel(
                    ?name 'performanceLabel
                    ?labelText "=== 성능 ==="
                )
                hiCreateIntField(
                    ?name 'cacheSize
                    ?prompt "캐시 크기 (MB)"
                    ?value configData["cacheSize"]
                )
                
                hiCreateButton(
                    ?name 'saveButton
                    ?buttonText "저장"
                    ?callback list("saveSettings()")
                )
                hiCreateButton(
                    ?name 'resetButton
                    ?buttonText "초기화"
                    ?callback list("resetSettings()")
                )
            )
        )
        
        hiDisplayForm(configForm)
    )
    
    procedure(loadSettings()
        ; 기본값 설정
        configData["gridSize"] = 0.005
        configData["autoSave"] = t
        configData["cacheSize"] = 100
        
        ; 파일에서 로드 (있으면)
        when(isFile("settings.il")
            load("settings.il")
        )
    )
    
    procedure(saveSettings()
        let((form port)
            form = hiGetCurrentForm()
            
            ; 폼에서 값 가져오기
            configData["gridSize"] = form->gridSize->value
            configData["autoSave"] = form->autoSave->value
            configData["cacheSize"] = form->cacheSize->value
            
            ; 파일에 저장
            port = outfile("settings.il" "w")
            when(port
                fprintf(port "; 설정 파일\n")
                fprintf(port "configData[\"gridSize\"] = %.6f\n"
                    configData["gridSize"])
                fprintf(port "configData[\"autoSave\"] = %L\n"
                    configData["autoSave"])
                fprintf(port "configData[\"cacheSize\"] = %d\n"
                    configData["cacheSize"])
                close(port)
            )
            
            showInfo("설정이 저장되었습니다")
        )
    )
    
    procedure(resetSettings()
        when(askYesNo("설정을 초기화하시겠습니까?")
            configData["gridSize"] = 0.005
            configData["autoSave"] = t
            configData["cacheSize"] = 100
            
            ; 폼 업데이트
            configForm->gridSize->value = 0.005
            configForm->autoSave->value = t
            configForm->cacheSize->value = 100
            
            showInfo("설정이 초기화되었습니다")
        )
    )
)
```

---

## GUI 디자인 팁

### 사용성

```
□ 명확한 레이블 사용
□ 논리적인 필드 순서
□ 적절한 기본값 제공
□ 입력 범위 제한
□ 에러 메시지 표시
```

### 레이아웃

```
□ 관련 필드 그룹화
□ 일관된 간격 유지
□ 중요한 요소 강조
□ 스크롤 최소화
□ 반응형 디자인 고려
```

### 성능

```
□ 무거운 작업은 별도 스레드
□ 진행 상황 표시
□ 입력 검증 최적화
□ 불필요한 업데이트 방지
```

---

## 일반적인 패턴

### 마법사 (Wizard)

```skill
let((wizardStep)
    wizardStep = 1
    
    procedure(showWizard()
        case(wizardStep
            (1 showStep1())
            (2 showStep2())
            (3 showStep3())
        )
    )
    
    procedure(nextStep()
        wizardStep = wizardStep + 1
        showWizard()
    )
    
    procedure(prevStep()
        wizardStep = wizardStep - 1
        showWizard()
    )
)
```

### 모달 대화상자

```skill
procedure(modalDialog(title message)
    let((result)
        result = hiDisplayAppDBox(
            ?name 'modalBox
            ?dboxBanner title
            ?dboxText message
            ?buttonLayout 'OKCancel
        )
        result
    )
)
```

### 툴팁

```skill
procedure(addTooltip(field text)
    field->help = text
)
```

---

## 관련 문서

- [[Virtuoso Skill 가이드]] - 기본 문법
- [[Virtuoso Skill 예제 모음]] - GUI 예제
- [[Virtuoso Skill 실전 레시피]] - 실전 활용

---

**좋은 GUI는 사용자가 도구를 의식하지 않게 만듭니다!**
