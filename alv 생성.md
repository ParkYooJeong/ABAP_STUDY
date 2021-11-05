## 1. INCLUE 생성


```

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

```

DATA : GV_CODE TYPE SY-UCOMM,
      OK_CODE TYPE SY-UCOMM.
```

## 5. TOP에 컨테이너 그리드 레이아웃 생성.

```
DATA : con1_dcok TYPE REF TO cl_gui_docking_container. " 컨테이너 선언
DATA : g_grid TYPE REF TO cl_gui_alv_grid. " 그리드
DATA : gs_layout TYPE lvc_s_layo. " 레이아웃 전체 화면 속성 정의
```

## 6. PBO INIT_SCREEN 작성

```
IF con1_dock IS INITIAL.

    CREATE OBJECT con1_dock
      EXPORTING
        repid     = sy-repid " Report to Which This Docking Control is Linked
        dynnr     = sy-dynnr " Screen to Which This Docking Control is Linked
        extension = 2000.   " Control Extension


    CREATE OBJECT G_GRID
      EXPORTING
        i_parent = CON1_DOCK. " 컨테이너 상속받은 그리드 생성

    CALL METHOD g_grid->set_table_for_first_display  "ALV 띄워주기위한 메소드 불러오기
```
