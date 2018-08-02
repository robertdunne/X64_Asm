;	Subroutine v_bin1 displays one byte from memory in binary.
;		R12: Points to the byte in memory
;		Registers preserved: RBX,RBP,RDI,RSI,RSP,R12-R15 

v_asc	proto			; Declare external subroutine.
	.code
v_bin1	proc			; Subroutine v_bin1 entry point
	push	RDI		; Save RDI and decrement RSP by 8
	mov	DL,[R12]	; Load byte to be displayed
	lea	RDI,bits8	; Pointer to ASCII display buffer
	cld			; String instructions will increment.
	mov	CL,7		; Bit 7 will be output first.

;	Loop through bits 7 to 0 converting them to ASCII.

nxtbit:	mov	AL,DL		; Copy byte to be displayed to AL.
	shr	AL,CL		; Shift current bit to bit 0.
	and	AL,1		; Mask off all bits except bit 0.
	add	AL,'0'		; Map binary 0,1 to ASCII '0','1'
	stosb			; Store in array of 8 "bits."
	dec	CL		; Number of bits left to process.
	jge	nxtbit		; Continue until all 8 bits done.
;
;	Display all 8 bits of current byte from memory buffer.
;
	lea	RDX,bits8	; Points to 8-byte memory buffer
	mov	R8,8		; Number of characters to display
	call	v_asc		; Subroutine that displays ASCII		

	pop	RDI		; Reload RDI and reposition stack
	ret			; Return to the calling program
v_bin1	endp

	.data
bits8	byte	8 DUP (?)	; Memory buffer for display
	end
