;	Subroutine v_dig1 displays one byte in a selected base.
;		R11: Contains the base (2 through 10)
;		R12: Points to the byte in memory
;		Registers preserved: RBX,RBP,RDI,RSI,RSP,R12-R15 

v_asc	proto			; Declare external subroutine.
	.code
v_dig1	proc			; Subroutine v_dig1 entry point
	push	RBX		; Save RBX and decrement RSP by 8
	mov	AL,[R12]	; Load byte to be displayed
	lea	RDX,dbuf+lengthof dbuf ; Point past buffer end.
	mov	R8,RDX		; (R8-RDX) will "count" digits.

;	Calculate next digit to be displayed.

modX:	dec	RDX		; The position to hold next digit
	mov	AH,0		; Prepare a 16-bit number in AX.
	div	R11B		; Get quotient in AL, remainder in AH.
	add	AH,'0'		; Map 0 through 9 to '0' through '9'
	mov	[RDX],AH	; Store in array of digits.
	and	AL,AL		; Test if any quotent left to process.
	jnz	modx		; Continue until all digits done.
;
;	Display all digits of current byte from memory buffer.
;
	sub	R8,RDX		; Number of characters to display
	call	v_asc		; Subroutine that displays ASCII		

	pop	RBX		; Reload RBX and reposition stack
	ret			; Return to the calling program
v_dig1	endp

	.data
dbuf	byte	8 DUP (?)	; Memory buffer for display
	end
