# Лабораторна робота №4.1

Завдання: переписати секцію Program мовою ST.

1) Добавити наступні змінні:

`TIC1_TRACING, TM_START` типу BOOL

2) Переписати секцію Program мовою ST

```pascal
SMPL1 (INTERVAL := T#100MS);
TIC1 (EN:=SMPL1.Q, PV := TE_Tank_R, SP := TIC1_SP, MAN_AUTO :=TIC1_MAN_ AUTO, 
PARA := TIC1_PARA, TR_S := TIC1_TRACING, OUT := VR_Par_R);
T_Delay (IN:=TM_START);

CASE StepProg OF
	0:
TIC1(TR_I:=0.0);
TIC1_TRACING:=TRUE;
	IF SB_Start THEN
		VA_nabor1:=TRUE;
		StepProg:=1;
	END_IF;
	1:
	IF LS_ser THEN
		VA_nabor1:=FALSE;
		VA_nabor2:=TRUE;
		StepProg:=2;
	END_IF;
	2:
	IF LS_verh THEN
		VA_nabor2:=FALSE;
		StepProg:=3;
	END_IF;
	3:
TIC1(TR_I:=100.0);
TIC1_TRACING:=TRUE;
	IF TE_Tank_R>=95.0 THEN
		StepProg:=4;
	END_IF;
	4:
TIC1(TR_I:=0.0);
TIC1_TRACING:=FALSE;
	TM_START:=1;
	IF T_Delay.Q THEN
		VA_sliv:=1;
		StepProg:=5;
		TM_START:=0;
TIC1.TR_I:= 0;
TIC1_TRACING:=TRUE;
	END_IF;
	5:
	IF NOT LS_nyz THEN
		VA_sliv:=FALSE;
		IF SB_Stop THEN
			StepProg:=0;
		ELSE
			VA_nabor1:=TRUE;
			StepProg:=1;
		END_IF;
	END_IF;

ELSE StepProg:=0;
END_CASE;
```

3) Добавити уставку таймера 3 хвилини.

4) Перенести виклик функціонального блоку T_Delay після TM_START:=1у кроці 4. Перевірити роботу програми протягом 2-3 циклів.

5) Змінити запуск SMPL1 з 5 циклу контролера.

6) Переписати секцію Inputs мовою ST.

 

## Контрольні питання:

1) Пояснити різницю між означеним і неозначеним викликом функції.

2) Що таке CASE? Його структура. Чим можна замінити CASE мовою FBD?

3) Чому необхідно проводити виклик функції постійно (чому таймер з пункту 4 працював некоректно)?

4) Яким чином записується присвоєння змінних типу INPUTS, OUTPUTS, INPUTS/OUTPUTS у функції?