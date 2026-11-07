# How to code STM32 using Arduino IDE

## Steps

- Open Arduino IDe
- Tools > Boards > Boards manager
- Add additional link https://raw.githubusercontent.com/stm32duino/BoardManagerFiles/main/package_stmicroelectronics_index.json
- Ok
- Boards > Search - STM32 microcontrollers by ST Microelectronics > Install board

## To upload code

- Tools > Boards > generic STM32F4 series
- Tools > Board part no > Black pill F401CC
- Tools > Upload method > STM32Cube Programmer(DFU)
- Tools > U(S)ART support > Enabled (generic 'Serial')
- Tools > USB support > CDC (general 'Serial' supersede U(S)ART)

### Write code normally how you do and then upload
- But everytime, do that boot0 + nrst thing
- The port will be disconnected, so select port as 1-1
