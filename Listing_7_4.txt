;	Subroutine v_hex1 displays one byte in hexadecimal.
;		R12: Points to the byte in memory
;		Registers preserved: RBX,RBP,RDI,RSI,RSP,R12-R15 

v_asc	proto			; Declare external subroutine.
	.code
v_hex1	proc			; Subroutine v_bin1 entry point
	push	RBX		; Save RBX and decrement RSP by 8
	lea	RDX,nib2	; Points to 2-byte memory buffer
	lea	RBX,dig		; Pointer to list of hex digits
	mov	AL,[R12]	; Load byte to be displayed
	shr	AL,4		; Right justify first nibble
	xlat			; Convert 4-bit nibble to hex digit
	mov	[RDX],AL	; Store high-order hex digit.

	mov	AL,[R12]	; Reload byte to be displayed
	and	AL,1111b	; Mask off all but second nibble.
	xlat			; Convert 4-bit nibble to hex digit
	mov	[RDX+1],AL	; Store low-order hex digit.

	mov	R8,2		; Number of characters to display
	call	v_asc		; Subroutine that displays ASCII		

	pop	RBX		; Reload RBX and reposition stack
	ret			; Return to the calling program
v_hex1	endp

	.data
dig	byte	"0123456789"	; ASCII string of digits 0 through 9
	byte	"ABCDEF"	; ASCII string of digits A through F
nib2	byte	2 DUP (?)	; Memory buffer for display
	end
