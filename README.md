#include "U8glib.h"

U8GLIB_SH1106_128X64 u8g(U8G_I2C_OPT_DEV_0 | U8G_I2C_OPT_FAST);

boolean boutonEtaitActif = false;
boolean ledEnabled = false;

void setup() {
  Serial.begin(9600);
  pinMode(10, OUTPUT);
  pinMode(2, INPUT_PULLUP);
  //clear_screen();
}

void loop() {
  boolean boutonActif = digitalRead(2); //boutonActif est de valeur 0 au tout début et a pour valeur 1 lorsque j'appuie
  //u8g.clear_screen();


  if (boutonEtaitActif && !boutonActif) {   // La led sallumera au moment ou on RELACHE le bouton car il faut que le bouton étaitf actif donc qu'on a appuyer dessus et la deuxieme condition evite que la led s'allume ou s'éteigne juste en restant appuyer. Il faut lacher le bouton
    delay(10); //pour pas qu'il y est des bugs

    boutonActif = digitalRead(2);

    if (!boutonActif) {
      //boutonetaitactif est toujours =1 ici
      ledEnabled = !ledEnabled;
      digitalWrite(10, ledEnabled);// allume/éteind led
      if (ledEnabled) {  //lorsque led s'allume
        u8g.firstPage();    // efface ecran
        do {
          u8g.setFont(u8g_font_unifont);

          u8g.drawStr( 0, 25, "biere blonde"); //(x,y, message)
        } while (u8g.nextPage());
      }

      if (!ledEnabled) {
        u8g.firstPage();    // efface ecran
        do {
          u8g.setFont(u8g_font_unifont);

          u8g.drawStr( 0, 25, "biere rousse"); //(x,y, message)
        } while (u8g.nextPage());
      }


    }

  }
  boutonEtaitActif = boutonActif; //sauvegarde de la valeur à 0



}# interrupteur_led_ecran
