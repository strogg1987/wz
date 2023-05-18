# wz
This is the WIZnet ioLibrary with minimal changes to make it work with libopencm3. Formatted as a library for Platformio
## usage
add to platformio.ini
```
lib_deps = https://github.com/strogg1987/wz
```
Add w5x00 related functions
```C
uint8_t spiReadByte(void)
{
    return spi_xfer(SPI1, 0xff);
}

void spiWriteByte(uint8_t byte)
{
    spi_xfer(SPI1, byte);
}

inline void csEnable(void)
{
    // GPIO_ResetBits(W5x00_CS_PORT, W5x00_CS_PIN);
    gpio_clear(WZ_CS_PORT, WZ_CS_PIN);
}

inline void csDisable(void)
{
    // GPIO_SetBits(W5x00_CS_PORT, W5x00_CS_PIN);
    gpio_set(WZ_CS_PORT, WZ_CS_PIN);
}

inline void resetAssert(void)
{
    // GPIO_ResetBits(W5x00_RESET_PORT, W5x00_RESET_PIN);
    gpio_clear(WZ_RST_PORT, WZ_RST_PORT);
}

inline void resetDeassert(void)
{
    gpio_set(WZ_RST_PORT, WZ_RST_PORT);
}

void W5x00Reset(void)
{    
    resetAssert();
    msleep(10);
    resetDeassert();
}
```
register callbacks
```C
reg_wizchip_cs_cbfunc(csEnable, csDisable);
reg_wizchip_spi_cbfunc(spiReadByte, spiWriteByte);
```
init wiznet chip in usual way
