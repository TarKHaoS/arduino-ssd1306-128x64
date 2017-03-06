# arduino-ssd1306-128x64
Arduino code from http://blog.alexweb.me/2015/ecran-oled-128x64-096-pour-arduino

---

Aujourd'hui j'ai reçu un petit écran OLED pour Arduino :

[![](http://i01.i.aliimg.com/wsphoto/v0/2053303145_1/0-96-Inch-I2C-IIC-Serial-font-b-128X64-b-font-font-b-OLED-b-font.jpg_220x220.jpg)<span style="display: block;">Ecran OLED 0.96" 128X64 blanc pour Arduino</span>](http://s.click.aliexpress.com/e/7q7eIqFE2)

Il s'agit donc d'un petit écran d'une résolution de 128 pixels de large par 64 pixels de haut, qui peut être suffisant pour certaines applications. Aussi, la technologie OLED est la plus confortable car chaque pixel est sa propre source de lumière, il n'y a donc pas de rétro-éclairage nécessaire.

J'ai utilisé la librairie U8Glib qui semble etre la plus simple: [https://code.google.com/p/u8glib/](https://code.google.com/p/u8glib/) 

Pour le câblage: 

 * VCC sur 3.3V 
 * GND sur GND 
 * SCL sur SCL 
 * SDA sur SDA

Simple, n'est-ce pas ? Sketch utilisé pour cette démo:

```Arduino
#include "U8glib.h"
U8GLIB_SSD1306_128X64 u8g(U8G_I2C_OPT_NO_ACK);  // Identifier for this display board

void draw(void) {
  // graphic commands to redraw the complete screen should be placed here  
  u8g.setFont(u8g_font_unifont);
  //u8g.setFont(u8g_font_osb21);
  u8g.drawStr( 0, 22, "Hello World!");
}

void setup(void) {
  // assign default color value
  if ( u8g.getMode() == U8G_MODE_R3G3B2 ) {
    u8g.setColorIndex(255);     // white
  }
  else if ( u8g.getMode() == U8G_MODE_GRAY2BIT ) {
    u8g.setColorIndex(3);         // max intensity
  }
  else if ( u8g.getMode() == U8G_MODE_BW ) {
    u8g.setColorIndex(1);         // pixel on
  }
  else if ( u8g.getMode() == U8G_MODE_HICOLOR ) {
    u8g.setHiColorByRGB(255,255,255);
  }
}

void loop(void) {
  // picture loop
  u8g.firstPage();  
  do {
    draw();
  } while( u8g.nextPage() );

  // rebuild the picture after some delay
  delay(50);
}
```

Et voilà !

[![IMG_20150519_184003](http://blog.alexweb.me/wp-content/uploads/2015/05/IMG_20150519_1840031-290x220.jpg)](http://blog.alexweb.me/wp-content/uploads/2015/05/IMG_20150519_1840031.jpg)