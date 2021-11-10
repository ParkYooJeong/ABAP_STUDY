## 1. INCLUE 생성


```ABAP
INCLUDE ZALV_HOMEWORKC01. -> 이벤트
INCLUDE ZALV_HOMEWORKO01                        .  " PBO-Modules
INCLUDE ZALV_HOMEWORKI01                        .  " PAI-Modules
INCLUDE ZALV_HOMEWORKF01          ->PERFORM


START-OF-SELECTION.
SELECT * FROM SFLIGHT INTO TABLE GT_SFLIGHT.
  CALL SCREEN 100.

```

## 2. 스크린생성.

OK_CODE생성
PBO, PAI 생성, STATUS 생성

## 3. BACK 생성. PAI 작성

## 4. TOP에 DATA 생성

```ABAP
DATA : GV_CODE TYPE SY-UCOMM,
      OK_CODE TYPE SY-UCOMM.
```

## 5. TOP에 컨테이너 그리드 레이아웃 생성.

```ABAP
DATA : con1_dock TYPE REF TO cl_gui_docking_container. " 도킹 컨테이너 선언
DATA : con1_ref  TYPE REF TO cl_gui_docking_container. " 커스텀 컨테이너 선언(둘중하나)
DATA : g_grid TYPE REF TO cl_gui_alv_grid. " 그리드
DATA : gs_layout TYPE lvc_s_layo. " 레이아웃 전체 화면 속성 정의
```

## 6. PBO INIT_SCREEN 작성

```ABAP
"도킹 컨테이너
"컨테이너 오브젝트가 한번 생성 되었으면다시 생성하지 않을 것.
IF con1_dock IS INITIAL.

" 컨테이너 오브젝트 생성해서 스크린 연결.
    CREATE OBJECT con1_dock 
      EXPORTING
        repid     = sy-repid " Report to Which This Docking Control is Linked
        dynnr     = sy-dynnr " Screen to Which This Docking Control is Linked
        extension = 2000.   " Control Extension

"alv grid 컨트롤 인스턴스 생성
    CREATE OBJECT G_GRID
      EXPORTING
        i_parent = CON1_DOCK. " 컨테이너 상속받은 그리드 생성

    CALL METHOD g_grid->set_table_for_first_display  "ALV 띄워주기위한 메소드 불러오기
        	EXPORTING
    		i_structure_name = 'SFLIGHT'  "  딕셔너리 구조체중에서 필드(구조)로 사용 할 구조체
    		"is_variant = "리스트의 변형  설정(필드 순서변경, 정렬 등)
    		"i_save = 'A' 레이아웃 사용자별 설정가능(기본 OR 유저) 
    		""'U' 는 유저 설정 '' 는 변경 가능 저장 불가
    		is_layout = gs_layout		" 레이아웃
    	CHANGING
    		it_outtab = gt_flight. 		" 화면에 보일 테이블
```

```ABAP
"커스텀 컨테이너일 경우
IF con1_ref IS INITIAL.

" 컨테이너 오브젝트 생성해서 스크린 연결.
    CREATE OBJECT con1_ref
      EXPORTING
        container_name = 'CON1'. "스크린 PAINTER에서 CUSTOM CONTROL로 만든 것의 NAME

"alv grid 컨트롤 인스턴스 생성
    CREATE OBJECT G_GRID
      EXPORTING
        i_parent = CON1_DOCK. " 컨테이너 상속받은 그리드 생성

    CALL METHOD g_grid->set_table_for_first_display  "ALV 띄워주기위한 메소드 불러오기
    	EXPORTING
    		i_structure_name = 'SFLIGHT'  "  딕셔너리 구조체중에서 필드(구조)로 사용 할 구조체
    		"is_variant = "리스트의 변형  설정(필드 순서변경, 정렬 등)
    		"i_save = 'A' 레이아웃 사용자별 설정가능(기본 OR 유저) 
    		""'U' 는 유저 설정 '' 는 변경 가능 저장 불가
    		is_layout = gs_layout		" 레이아웃
    	CHANGING
    		it_outtab = gt_flight. 		" 화면에 보일 테이블
```



## 7. LAYOUT 설정

```ABAP
FORM setting_layout CHANGING p_layout TYPE lvc_s_layo.
	p_layout-edit = 'X' . "수정가능 필드 모드(전체)
	
ENDFORM.
```

