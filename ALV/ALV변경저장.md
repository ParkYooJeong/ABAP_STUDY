# ALV 변경저장

[TOC]



## 1. 레이아웃 설정 or 필드 카탈로그 속성 설정

```ABAP
FORM setting_layout CHANGING p_layout TYPE lvc_s_layo.
	p_layout-edit = 'X' . "수정가능 필드 모드(전체)
	
ENDFORM.
```



## 2. 이벤트 등록

```abap
DATA : gt_modified_rows like table of gt_sflight. 
```



```ABAP
 CLASS lcl_event_receiver DEFINITION.
    PUBLIC SECTION.
      METHODS:
      handle_data_changed
      FOR EVENT data_changed of cl_gui_alv_grid
        IMPORTING er_data_changed.
  ENDCLASS.

  CLASS lcl_event_receiver IMPLEMENTATION.
    METHOD handle_data_changed.
      DATA: ls_sflight TYPE sflight,
            ls_outtab LIKE LINE OF gt_sflight.

      FIELD-SYMBOLS: <fs> TYPE table.
      ASSIGN er_data_changed->mp_mod_rows->* TO <fs>.
      LOOP AT <fs> INTO ls_outtab.
        MOVE-CORRESPONDING ls_outtab to ls_sflight.
        APPEND ls_sflight to gt_modified_rows.
      ENDLOOP.
    ENDMETHOD.
  ENDCLASS.

```



이벤트 핸들러 o에 등록

i에서 save 누를시 이벤트 호출

update_database perform 생성

```abap
modify sflight from table gt_modified_rows.
```

