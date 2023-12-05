# Embedded-Systems
Designed and Simulated an Embedded System circuit using Proteus an Automatic Street Light controller that block of street light ahead of it and simultaneously switching off the trailing lights by detecting the vehicle movement on highways to save energy.

Assembly Code:
org   0000h    
jmp   MAIN 
org  0400h
DB 000H, 001H, 003H, 007H, 00EH, 01CH, 038H, 070H, 0E0H
MAIN : MOV DPTR,#0400H ; ADDRESS OF TABLE TO DPTR 
           MOV P1,#0FFH ;SETTING P1 FOR INPUT
           MOV P2,#00H  ;SETTING P2 FOR OUTPUT      
           
CHECK:	CLR PSW.7  ;CLEARING THE CARRY FLAG        
              MOV A,P1   ;STORING INPUT IN THE ACC	
              MOV R2,#00H ;INTIALIZING COUNTER	
              CJNE A,#00H,LOOP ;CHECK WHETHER ANY SENSOR IS ON	
              MOV P2,#00H      ;IF NOT THEN DISPLAY "0"	
              SJMP CHECK       ;CHECK AGAIN UNTIL A SENSOR IS TURNED ON
LOOP:  RRC A       ;ROTATE ACC UNTIL CARRY IS ONE        
           INC R2      ;COUNT WHICH SENSOR IS WORKING	
          JNC LOOP    ;CONTINUE TILL CARRY IS ZERO
OUTPUT:	MOV A,R2    ;MOVE THE COUNT OF SENSOR IN ACC	
              MOVC A,@A+DPTR  ;STORE THE VALUE FOR THE CORRESPONDING SENSOR FROM LOOKUP TABLE	;
              MOV P2,A     ;GIVE VALUE TO P2 AS OUTPUT		
              LJMP CHECK  ;CONTINUE THE ABOVE PROCESS LIFETIME 
