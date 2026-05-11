esphome - megvan
esphome builder kell még
esp-homebuildernél:
	- new device
	- new device setup
	- név bármi amit jónak találsz
- installation: 
	skip this step
	beállítod az eszközöd (esp8266)
	Install, majd cancel
	
létrejött az általad elnevezett XY.yaml egy kártyán látod az esphome felületén.

A kártyán válaszd az Edit-et.

Nagyon fontos:
az esp8266:
  board: esp01_1m eszköz helyére ezt írd: board: d1_mini

a captive_portal:
után ezt illeszd be:


# --- OPENTHERM ALAPBEÁLLÍTÁS ---
opentherm:
  in_pin: D2  # Wemos D2
  out_pin: D1 # Wemos D1

# --- SZENZOROK (Adatgyűjtés a kazánból) ---
sensor:
  - platform: opentherm
    t_boiler:
      name: "Kazán Előremenő Hőmérséklet"
    t_ret:
      name: "Visszatérő Hőmérséklet"
    
    # rel_mod_lvl helyett rel_mod_level-t kér:
    rel_mod_level: 
      name: "Kazán Aktuális Moduláció"
      unit_of_measurement: "%"
    
    # pressure helyett ch_pressure-t kér:
    ch_pressure: 
      name: "Rendszernyomás"
      unit_of_measurement: "bar"
      
    t_outside:
      name: "Kazán által érzékelt külső hőm"


# --- VEZÉRLÉS (Ezeket fogja a PID állítani a HA-ból) ---
number:
  - platform: opentherm
    # setpoint_temperature helyett t_set
    t_set:
      name: "Fűtési víz alapjel (CH Setpoint)"
      initial_value: 40
      min_value: 20
      max_value: 75
      step: 0.5
    
    # hot_water_setpoint_temperature helyett t_dhw_set
    t_dhw_set:
      name: "Melegvíz alapjel (DHW Setpoint)"
      initial_value: 45
      min_value: 35
      max_value: 65
      step: 1

# --- ÁLLAPOTJELZŐK (Diagnosztika) ---
binary_sensor:
  - platform: opentherm
    flame_on:
      name: "Gázláng"
    # central_heating_active helyett ch_active
    ch_active:
      name: "Fűtés aktív"
    # hot_water_active helyett dhw_active
    dhw_active:
      name: "Melegvíz készítés aktív"
    # fault helyett flame_fault vagy fault_indication (a logod flame_fault-ot sugall)
    flame_fault:
      name: "Kazán Hiba"
    diagnostic_indication:
      name: "Szerviz jelzés"

# --- KAPCSOLÓK ---
switch:
  - platform: opentherm
    ch_enable:
      name: "Fűtés Engedélyezése"
    dhw_enable:
      name: "Melegvíz Engedélyezése"


majd mentsd
utána a kártyán a 3 pöttynél válaszd az installt.
majd kábellel össze kell kötnöd a géped a wemossal.
válaszd a Plug into this computer-t
itt kiválasztod a COM-portot ahol az eszköz van. 
vársz és jó lesz.

Azt majd nézd meg, hogy a Secret nevű fájl létre jött-e mikor az esphomebuildert megcsináltad.

Ha megakadsz írj.
