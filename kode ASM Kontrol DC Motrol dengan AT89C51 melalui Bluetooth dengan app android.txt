ORG 00H
	
	MOV TMOD,#20H       ; Timer 1 Mode 2, 8 bit mode
	MOV TH1,#0FDH       ; set Baud Rate menjadi 9600
	MOV SCON,#50H       ; Serial Mode 1 10 bit total 1sn, 8 db, 1STOPb
	CLR TI              ; membuat register TI menjadi 0
	CLR RI              ; membuat register RI menjadi 0
	MOV P1,#00000000B   ; mengosongkan P1 agar motor kiri berhenti dengan sendirinya
        MOV P2,#00000000B   ; mengosongkan P2 agar motor kanan berhenti dengan sendirinya
	
	SETB TR1            ; memulai TIMER 1
	
	AGAIN:              ;Label 'AGAIN' sebagai keadaan awal sistem
	  CLR RI              ;membuat register RI menjadi 0
	 repeat:            ;Label repeat sebagai keadaan netral sistem 
	   JNB RI, repeat   ;apabila register RI adalah'0' maka akan tetap pada keadaan netral
	   MOV A,SBUF       ;memindahkan isi register SBUF ke register A
	   
	   CJNE A,#'A',checknext ;memeriksa apakah nilai  di register A adalah String 'A'
	   ;MAJU                 ;Apabila isi register A adalah string 'A' maka dijalankan program dibawahnya
	   MOV P1,#00000001B     ;membuat DC Motor kiri berputar searah Jarum Jam
           MOV P2,#00000001B     ;membuat DC Motor kanan berputar searah Jarum Jam
           ACALL DELAY           ;mwmanggil fungsi delay
           MOV P1,#00000000B     ;membuat DC Motor kiri berhenti
           MOV P2,#00000000B     ;membuat DC Motor kanan berhenti
	   SJMP AGAIN            ;setelah instruksi dijalankan loncat ke keadaan awal
	   
        checknext:              ;label/fungsi untuk mundur
	   CJNE A,#'a',checknext2;memeriksa apakah nilai  di register A adalah String 'A'
	   ;MUNDUR               ;Apabila isi register A adalah string 'a' maka dijalankan program dibawahnya
	   MOV P1,#00000010B    ;membuat DC Motor kiri berputar berlawanan arah jarum jam
           MOV P2,#00000010B    ;membuat DC Motor kanan berputar berlawanan arah jarum jam
           ACALL DELAY          ;memanggil delay
           MOV P1,#00000000B    ;membuat DC Motor kiri berhenti
           MOV P2,#00000000B    ;membuat DC Motor kanan berhenti
	   SJMP AGAIN           ;setelah instruksi dijalankan loncat ke keadaan awal
	 
         checknext2:
	   CJNE A,#'B',checknext3;memeriksa apakah nilai  di register A adalah String 'A'
	   ;KIRI                  ;Apabila isi register A adalah string 'B' maka dijalankan program dibawahnya
	   MOV P1,#00000001B      ;membuat DC Motor kiri berputar searah arah jarum jam
           MOV P2,#00000010B      ;membuat DC Motor Kanan berputar berlawanan arah jarum jam
           ACALL DELAY            ;memanggil delay
           MOV P1,#00000000B      ;membuat DC Motor kiri berhenti
           MOV P2,#00000000B      ;membuat DC Motor kanan berhenti
	   SJMP AGAIN             ;setelah instruksi dijalankan loncat ke keadaan awal
	 
	 checknext3:
	   CJNE A,#'b', AGAIN     ;memeriksa apakah nilai  di register A adalah String 'A'
	   ;KANAN                 ;Apabila isi register A adalah string 'b' maka dijalankan program dibawahnya
           MOV P1,#00000010B      ;membuat DC Motor Kiri berputar berlawanan arah jarum jam
           MOV P2,#00000001B      ;membuat DC Motor Kanan berputar searah arah jarum jam
           ACALL DELAY            ;memanggil label/fungsi delay
           MOV P1,#00000000B      ;membuat DC Motor kiri berhenti
           MOV P2,#00000000B      ;membuat DC Motor kanan berhenti
	   SJMP AGAIN             ;setelah instruksi dijalankan loncat ke keadaan awal
	   RET
	   
DELAY: MOV R4,#0FH       ;fungsi Delay
WAIT1: MOV R3,#00H
WAIT2: MOV R2,#00H
WAIT3: DJNZ R2,WAIT3
DJNZ R3,WAIT2
DJNZ R4,WAIT1            ;akhir fungsi delay  
END