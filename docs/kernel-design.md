# Complete system architecture

## Layered design

HAL-OS is divided into the following layers:

1. Hardware
   - W65C02 CPU
   - ROM, RAM, bank switching
   - VGA, Ethernet, SD, audio, keyboard, mouse
2. Hardware abstraction layer
   - device drivers
   - interrupt handlers
   - I/O register wrappers
3. Kernel
   - scheduler
   - memory manager
   - IPC
   - tasking
   - device manager
4. System services
   - file system client
   - service registry
   - configuration system
   - event bus
5. Desktop environment
   - window manager
   - taskbar
   - input manager
   - desktop compositor
6. Applications
   - terminal
   - editor
   - calculator
   - file manager
   - music player
   - browser
7. Development tools
   - assembler
   - debugger
   - HAL SDK
   - HALScript

## Kernel responsibilities

- boot process
- task scheduling
- memory allocation
- drivers and hardware control
- IPC message routing
- file system access
- system calls
- interrupt handling

## Device model

Each device implements the same abstract interface:

- init
- read
- write
- ioctl
- interrupt
- reset

## Process model

- max tasks: 32
- cooperative multitasking
- tasks can yield or block
- each task owns a message queue
- tasks interact through IPC

## System call interface

```text
SYS_EXIT    = $01
SYS_YIELD   = $10
SYS_OPEN    = $20
SYS_READ    = $21
SYS_WRITE   = $22
SYS_CLOSE   = $23
SYS_SEND    = $30
SYS_RECV    = $31
SYS_ALLOC   = $40
SYS_FREE    = $41
```

## System bus

The system bus carries packets between components:

```text
+-------------------+
| CPU               |
+---------+---------+
          |
          v
+---------+---------+
| Memory Manager    |
+---------+---------+
          |
  +-------+--------+
  | Driver Manager |
  +-------+--------+
          |
  +-------+-----------------+
  | Kernel Services         |
  +------------------------+
```

## Security model

- privilege ring model using kernel/user separation
- system calls gate kernel access
- file permissions for read/write/execute
- memory ownership flags for tasks

## Boot sequence

1. CPU reset
2. ROM startup code runs
3. hardware detection
4. RAM bank validation
5. SD card scan
6. kernel image load
7. kernel jump
8. init drivers
9. start scheduler
10. launch desktop shell
