# Chip8-Emulator
A CHIP-8 emulator works by simulating a virtual machine that contains memory, registers, stack, timers, display, and keypad. CHIP-8 architecture consists of 4KB memory, 16 general purpose 8-bit registers, a 16-bit index register, a program counter, stack and stack pointer, delay timer and sound timer, a 64×32 pixel display, and a 16-key keypad. The program ROM is loaded into memory starting at address 0x200. The CPU executes instructions using a fetch–decode–execute cycle where each instruction is 2 bytes long. Some instructions perform arithmetic operations, some handle jumps and subroutines using the stack, some draw sprites to the display, and some handle keypad input. The emulator continuously runs this cycle to execute the game program.

1. 0x000 to 0x1FF (first 512 bytes) was reserved for the original CHIP-8 interpreter
2. The game programs were loaded after that, starting from address 0x200.

<img width="1292" height="674" alt="image" src="https://github.com/user-attachments/assets/e48d5a89-eb58-4482-82d3-02c9ebcf556c" />

# Complete Flow of Emulator
1. Initialize CHIP-8 system. 
2. Load ROM into memory starting at 0x200. 
3. Start emulation loop. 
4. Fetch opcode from memory using PC. 
5. Decode opcode using bit masking. 
6. Execute instruction. 
7. Update timers. 
8. Update display buffer. 
9. Handle keypad input. 
10. Repeat loop until program ends.


# Step 1:- Create CHIP-8 System Structure
First, we create a structure that represents the whole CHIP-8 machine. This structure will contain memory, registers, stack, program counter, index register, timers, display buffer, and keypad state

In simple words, we are creating a software model of a small computer.
The CHIP-8 system contains:
1. 4096 bytes memory 
2. 16 registers (V0–VF) 
3. Index register (I) 
4. Program counter (PC) 
5. Stack 
6. Stack pointer 
7. Delay timer 
8. Sound timer 
9. Display buffer (64×32) 
10. Keypad (16 keys) 
11. Current opcode 
This structure represents the complete virtual machine.

# Step 2 – Initialize the Emulator
After creating the structure, we initialize everything:
	1. Clear memory 
	2. Clear registers 
	3. Clear display 
	4. Set PC = 0x200 
	5. Load fontset into memory 
	6. Reset timers 
	7. Reset stack pointer 
The reason PC starts from 0x200 is because the first 512 bytes were reserved for interpreter in old systems, so programs start from 0x200.
So initialization basically prepares the virtual machine to run a game ROM.

# Step 3 – Load the ROM (Game Program)
Next step is to load the CHIP-8 game file (ROM) into memory starting from address 0x200.
So memory layout becomes:
	1. 0x000–0x1FF → Reserved 
	2 0x200–... → Game ROM 
After loading ROM, the emulator is ready to execute instructions.

# Step 4 – Emulation Cycle (Most Important Part)
This is the heart of the emulator. The emulator runs in a loop and performs these steps repeatedly:
	1. Fetch opcode from memory 
	2. Decode opcode 
	3. Execute opcode 
	4. Update timers 
	5. Update display 
	6. Handle input 
	7. Move to next instruction 
This is exactly how a real CPU works. This loop runs many times per second.
This cycle is called the Fetch–Decode–Execute cycle, and this is very important for interviews.

# Step 5 – Fetch Opcode
Each CHIP-8 instruction is 2 bytes.
So we read two consecutive memory locations and combine them into one opcode.
Example concept:
	1. Read memory[PC] 
	2. Read memory[PC+1] 
	3. Combine into opcode 
	4. PC = PC + 2 
This means we fetched the next instruction.

# Step 6 – Decode Opcode
Now we check what instruction it is. CHIP-8 instructions are identified by their pattern.
For example:
	1. 00E0 → Clear screen 
	2. 1NNN → Jump 
	3. 6XNN → Set register 
	4. 7XNN → Add value 
	5. ANNN → Set index register 
	6. DXYN → Draw sprite 
	7. EX9E → Key pressed 
	8. FX15 → Set delay timer 
So we use switch case or bit masking to decode opcode and determine which instruction it is.
Decoding is basically understanding what the instruction wants the system to do.

# Step 7 – Execute Opcode
After decoding, we execute the instruction.
Examples:
	1. Clear screen → set display array to zero 
	2. Jump → change PC 
	3. Set register → store value in Vx 
	4. Add → add value to register 
	5. Draw → draw pixels on screen buffer 
	6. Key press → check keypad array 
	7. Call function → push PC to stack 
	8. Return → pop PC from stack 
This part is called instruction execution.

# Step 8 – Timers
CHIP-8 has two timers:
	1. Delay timer 
	2. Sound timer 
They decrease at 60 Hz until they reach zero.
When sound timer becomes non-zero, system makes a beep sound.
So in emulator loop, we decrease timers regularly.

# Step 9 – Display (Graphics)
Display is 64 × 32 pixels.
We maintain a display buffer array. When the draw opcode runs, we update pixels in this buffer. Then we render this buffer on screen using SDL or any graphics library.
So graphics in CHIP-8 are basically turning pixels ON or OFF.

# Step 10 – Input (Keypad)
CHIP-8 has 16 keys (0–F).
We store keypad state in an array. When a key is pressed, we update that array. Some instructions check whether a key is pressed and act accordingly.
So input handling is just checking keypad array values.
