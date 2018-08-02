includelib kernel32.lib		; Windows kernel interface
GetStdHandle	proto		; Function to retrieve I/O handle
WriteConsoleA	proto		; Function that writes to command window
Console		equ	-11	; Device code for console text output.
ExitProcess	proto

	.code
mainCRTStartup	proc

	sub	RSP,40		; Reserve "shadow space" on stack.

;	Obtain "handle" for console display monitor I/O streams

	mov	RCX,Console	; Console standard output handle
	call	GetStdHandle	; Returns handle in register RAX
	mov	stdout,RAX	; Save handle for text display.

;	Display the "Hello World" message.

	mov	RCX,stdout	; Handle to standard output device 
	lea	RDX,hwm		; Pointer to message (byte array).
	mov	R8,lengthof hwm ; Number of characters to display
	lea	R9,nbwr		; Number of bytes actually written.
	call	WriteConsoleA	; Write text string to window.

	add	RSP,40		; Replace "shadow space" on stack
	mov	RCX,0		; Set exit status code to zero.
	call	ExitProcess	; Return control to Windows.

mainCRTStartup	endp

	.data
hwm	byte	"Hello World"
stdout	qword	?		; Handle to standard output device
nbwr	qword	?		; Number of bytes actually written

	end