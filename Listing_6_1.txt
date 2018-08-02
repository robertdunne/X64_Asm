;	Subroutines to display one or more characters on the console.
;		v_opn: Opens the standard display monitor 
;		v_asc: Displays string of characters in memory buffer 
;		v_asc1: Displays one characters in memory buffer 

	includelib kernel32.lib	; Windows kernel interface
GetStdHandle	proto		; Command to retrieve I/O handle
WriteConsoleA	proto		; Command to write to command window
Console		equ	-11	; Command code for monitor handle.
		.code

;	Subroutine v_opn will open the standard display monitor.
;		Registers preserved: RBX,RBP,RDI,RSI,RSP,R12-R15 

v_opn	proc
	mov	RCX,Console	; Console standard display monitor handle
	sub	RSP,40		; Reserve "shadow" and 16 byte align.
	call	GetStdHandle	; Get console display handle
	add	RSP,40		; Reset stack to return address.
	mov	stdout,RAX	; Save handle of display monitor stream.
	ret			; Return to the calling program.
v_opn endp

;	Subroutine v_asc will display characters in memory buffer.
;		RDX: Points to first character in memory
;		R8:  Number of bytes to display
;		RSP: 16-byte aligned before CALL
;		Registers preserved: RBX,RBP,RDI,RSI,RSP,R12-R15 

v_asc proc
	lea	R9,nbwr		; Returned with count of bytes written.
	mov	RCX,stdout	; I/O handle for display monitor.
	sub	RSP,40		; Reserve "shadow" and 16 byte align.
	call	WriteConsoleA	; Ask Windows to write to monitor.
	add	RSP,40		; Reset stack to return address.
	ret			; Return to the calling program.
v_asc endp

;	Subroutine v_asc1 will display 1 character in memory buffer.
;		R12: Points to the one character in memory
;		RSP: 16-byte aligned before CALL
;		Registers preserved: RBX,RBP,RDI,RSI,RSP,R12-R15 

v_asc1 proc
	mov	R8,1		; Number of bytes requested to write.
	lea	R9,nbwr		; Returned with count of bytes written.
	mov	RDX,R12		; Memory address of buffer to write.
	mov	RCX,stdout	; I/O handle for display monitor.
	sub	RSP,40		; Reserve "shadow" and 16 byte align.
	call	WriteConsoleA	; Ask Windows to write to monitor.
	add	RSP,40		; Reset stack to return address.
	ret			; Return to the calling program.
v_asc1 endp

	.data
stdout	qword	?		; Handle to standard output device
nbwr	qword	?		; Number of bytes written
	end