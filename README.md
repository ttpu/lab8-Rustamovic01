    AREA Start, CODE, READONLY
    ENTRY

    MOV     R0, #0                   ; Initialize counter to 0
    LDR     R1, =0x40000000          ; Load base address of the result array

generate_even:
    STR     R0, [R1], #4             ; Store the current value of R0 in memory (increment address by 4)
    ADD     R0, R0, #2               ; Increment R0 by 2 to get the next even number
    CMP     R0, #20                  ; Check if R0 >= 20
    BLT     generate_even            ; If less, continue generating even numbers

    STOP    B STOP                   ; Infinite loop to stop execution

    END

    AREA Start, CODE, READONLY
    ENTRY

    LDR     R0, =array               ; Load address of array into R0
    MOV     R1, #0                   ; Initialize sum to 0
    MOV     R2, #4                   ; Array size (4 elements)

sum_array:
    LDR     R3, [R0], #4             ; Load an element from the array into R3 and increment R0
    ADD     R1, R1, R3               ; Add R3 to sum (R1)
    SUB     R2, R2, #1               ; Decrease size by 1
    CMP     R2, #0                   ; Check if all elements are processed
    BNE     sum_array                ; If not, repeat the loop

    STOP    B STOP                   ; Infinite loop to stop execution

    END

data
array   DCD 1, 2, 3, 4              ; Array of 4 elements (1, 2, 3, 4)

    AREA Start, CODE, READONLY
    ENTRY

    LDR     R0, =array               ; Load address of array into R0
    MOV     R1, #4                   ; Array size (4 elements)
    LDR     R2, [R0], #4             ; Load the first element into R2 (min)
    MOV     R3, R2                   ; Set R3 (max) to the first element

min_max:
    LDR     R4, [R0], #4             ; Load the next element into R4
    CMP     R4, R2                   ; Compare R4 with the current min (R2)
    MOVLT   R2, R4                   ; If R4 < R2, update R2 (min)
    CMP     R4, R3                   ; Compare R4 with the current max (R3)
    MOVGT   R3, R4                   ; If R4 > R3, update R3 (max)
    SUB     R1, R1, #1               ; Decrease remaining array size
    CMP     R1, #0                   ; Check if we are done
    BNE     min_max                  ; If not, continue the loop

    STOP    B STOP                   ; Infinite loop to stop execution

    END

data
array   DCD 5, 10, 2, 8             ; Array with 4 elements (5, 10, 2, 8)
