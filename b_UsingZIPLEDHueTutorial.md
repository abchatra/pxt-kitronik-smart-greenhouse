### @activities true
### @explicitHints true

# Displaying Temperature, Humidity & Soil Moisture

## Temperature on a Single LED
### Introduction Step @unplugged
In the "Greenhouse Visual Thermometer" tutorial (click [here](https://makecode.microbit.org/#tutorial:https://github.com/KitronikLtd/pxt-kitronik-ec-board/a_VisualThermometerTutorial) to view) all the ZIP LEDs were used to display the temperature. However, this isn't always very helpful as there are other measurements it would be good to display, such as humidity and soil moisture.

In this tutorial, learn how to use the ZIP LED hue feature to display data on a single status LED.

![Ticking Halo HD Clock animation](https://KitronikLtd.github.io/pxt-kitronik-halohd/assets/Ticking-Clock-Animation.gif)

### Step 1
The first thing to do is to set up the ZIP LEDs and create the status LEDs range.
From the ``||kitronik_environmental_board.ZIP LEDs||`` section of the ``||kitronik_environmental_board.Environmental||`` category, add the ``||variables:set zipLEDs to||`` ``||kitronik_environmental_board.Environmental Board with 3 ZIP LEDs||`` block to the ``||basic:on start||`` section, followed by the ``||variables:set statusLEDs||`` block.

#### ~ tutorialhint
```blocks
let zipLEDs = kitronik_environmental_board.createECZIPDisplay(3)
let statusLEDs = zipLEDs.statusLedsRange()
```

### Step 2
Next, create a new variable called ``||variables:tempHue||``. This will be used to set and store the ZIP LED colour hue based on the temperature reading from the sensor.  
In the ``||basic:forever||`` loop, ``||variables:set tempHue to||`` ``||math:map 0 from low 0 high 1023 to low 0 high 4||`` - this block can be found in the ``||math:Math||`` category.

#### ~ tutorialhint
```blocks
basic.forever(function () {
    tempHue = Math.map(0, 0, 1023, 0, 4)
})
```

### Step 3
The ``||math.map||`` function changes the possible range of a value - in this case temperature - to another range, changing in relative amounts. This is very useful, as it means temperature, measured between 0 and 40°C, can then be converted to a colour hue value (these range from 0-360, red all the way round a colour wheel to red again).  
In the ``||math:map||`` block, put the ``||kitronik_environmental_board.Read Temperature in °C||`` block in the first slot - this can be found in the ``||kitronik_environmental_board.Sensors||`` section of the ``||kitronik_environmental_board.Environmental||`` category. Then, set ``||math:from low 0 high 40 to low 210 high 0||`` to give blue colours for cold, and red colours for hot.

#### ~ tutorialhint
```blocks
basic.forever(function () {
    tempHue = Math.map(kitronik_environmental_board.temperature(TemperatureUnitList.C), 0, 40, 210, 0)
})
```

### Step 4
Now that the temperature hue has been set and stored, the ZIP LED colour can now be set.  
After the ``||variables:set tempHue||`` block, add a  ``||kitronik_environmental_board.set ZIP LED 0 to red||`` block from the  ``||kitronik_environmental_board.ZIP LEDs||`` section of the  ``||kitronik_environmental_board.Environmental||`` category (remember to change the variable drop-down to be ``||variables:statusLEDs||``). Instead of selecting a colour from the drop-down list, insert a  ``||kitronik_environmental_board.hue 0||`` block into the slot, and put the variable ``||variables:tempHue||`` in place of the 0.  
Finally, put a ``||variables:statusLEDs||``  ``||kitronik_environmental_board.show||`` block at the end of the ``||basic:forever||`` loop.

#### ~ tutorialhint
```blocks
let statusLEDs: kitronik_environmental_board.ecZIPLEDs = null
basic.forever(function () {
    tempHue = Math.map(kitronik_environmental_board.temperature(TemperatureUnitList.C), 0, 40, 210, 0)
    statusLEDs.setZipLedColor(0, kitronik_environmental_board.hueToRGB(tempHue))
    statusLEDs.show()
})
```

### Step 5
Click ``|Download|`` and transfer the code to the Environmental Control Board.  
Try putting the board in locations with different temperatures to see the ZIP LED colour change.

## Temperature, Humidity & Soil Moisture
### Introduction Step @unplugged
Now that temperature is working, humidity and soil moisture can be added to ZIP LEDs 1 and 2.  
  
This stage of the tutorial is going to require the Prong to be connected to the Environmental Control Board using crocodile clips.  
Connect Prong 3V to one of the 3V pads, Prong GND to one of the GND pads and Prong P1 to the Pin1 pad. Stick it into a plant pot.

![Picture of Prong connected to Environmental Control Board]

### Step 1
Create two more new variables, one called ``||variables:humidHue||`` and the other ``||variables:soilHue||``.  
Directly after the ``||variables:set tempHue||`` block in the ``||basic:forever||`` loop, set both of the new variables to the ``||math:map||`` block (as for ``||variables:tempHue||``).

#### ~ tutorialhint
```blocks
let statusLEDs: kitronik_environmental_board.ecZIPLEDs = null
basic.forever(function () {
    tempHue = Math.map(kitronik_environmental_board.temperature(TemperatureUnitList.C), 0, 40, 210, 0)
    humidHue = Math.map(0, 0, 1023, 0, 4)
    soilHue = Math.map(0, 0, 1023, 0, 4)
    statusLEDs.setZipLedColor(0, kitronik_environmental_board.hueToRGB(tempHue))
    statusLEDs.show()
})
```

### Step 2
Similar to ``||variables:tempHue||``, ``||variables:set humidHue to||`` ``||math:map||`` ``||kitronik_environmental_board.Read Humidity||`` - this block can be found in the ``||kitronik_environmental_board.Sensors||`` section of the ``||kitronik_environmental_board.Environmental||`` category. The mapping values should be: ``||math:from low 0 high 100 to low 35 high 150||``. This will give a desert sand colour for low humidity and a watery colour for high humidity.

#### ~ tutorialhint
```blocks
let statusLEDs: kitronik_environmental_board.ecZIPLEDs = null
basic.forever(function () {
    tempHue = Math.map(kitronik_environmental_board.temperature(TemperatureUnitList.C), 0, 40, 210, 0)
    humidHue = Math.map(kitronik_environmental_board.humidity(), 0, 100, 35, 150)
    soilHue = Math.map(0, 0, 1023, 0, 4)
    statusLEDs.setZipLedColor(0, kitronik_environmental_board.hueToRGB(tempHue))
    statusLEDs.show()
})
```

### Step 3
Now do the same thing for ``||variables:soilHue||``, but this time ``||math:map||`` ``||kitronik_environmental_board.Analog read P1||`` - this block can be found in the ``||kitronik_environmental_board.Inputs/Outputs||`` section of the ``||kitronik_environmental_board.Environmental||`` category. The mapping values should be: ``||math:from low 0 high 1023 to low 35 high 150||``. This will give a desert sand colour for a low moisture reading and a watery colour for a high moisture reading.

#### ~ tutorialhint
```blocks
let statusLEDs: kitronik_environmental_board.ecZIPLEDs = null
basic.forever(function () {
    tempHue = Math.map(kitronik_environmental_board.temperature(TemperatureUnitList.C), 0, 40, 210, 0)
    humidHue = Math.map(kitronik_environmental_board.humidity(), 0, 100, 35, 150)
    soilHue = Math.map(kitronik_environmental_board.readIOPin(kitronik_environmental_board.PinType.analog, kitronik_environmental_board.IOPins.p1), 0, 1023, 35, 150)
    statusLEDs.setZipLedColor(0, kitronik_environmental_board.hueToRGB(tempHue))
    statusLEDs.show()
})
```

### Step 4
There are now hue values linked to each of the sensor readings, so the next thing to do is to link the other status LEDs to the readings.  
Just before the ``||kitronik_environmental_board.show||`` block, add in two more ``||variables:statusLEDs||`` ``||kitronik_environmental_board.set ZIP LED #||`` blocks - one setting ``||kitronik_environmental_board.ZIP LED 1 to hue||`` ``||variables:humidHue||`` and the other setting ``||kitronik_environmental_board.ZIP LED 2 to hue||`` ``||variables:soilHue||``.

#### ~ tutorialhint
```blocks
let statusLEDs: kitronik_environmental_board.ecZIPLEDs = null
basic.forever(function () {
    tempHue = Math.map(kitronik_environmental_board.temperature(TemperatureUnitList.C), 0, 40, 210, 0)
    humidHue = Math.map(kitronik_environmental_board.humidity(), 0, 100, 35, 150)
    soilHue = Math.map(kitronik_environmental_board.readIOPin(kitronik_environmental_board.PinType.analog, kitronik_environmental_board.IOPins.p1), 0, 1023, 35, 150)
    statusLEDs.setZipLedColor(0, kitronik_environmental_board.hueToRGB(tempHue))
    statusLEDs.setZipLedColor(1, kitronik_environmental_board.hueToRGB(humidHue))
    statusLEDs.setZipLedColor(2, kitronik_environmental_board.hueToRGB(soilHue))
    statusLEDs.show()
})
```

### Step 5
The final stage is to add the functionality to display the actual numerical sensor readings on the micro:bit LEDs.  
From the ``||input:Input||`` category, add in three ``||input:on button press||`` blocks - one for ``||input:button A||``, one for ``||input:button B||`` and one for ``||input:button A+B||``. Using the ``||basic:show number||`` block, make ``||input:A||`` show ``||kitronik_environmental_board.Read Temperature||``, ``||input:B||`` show ``||kitronik_environmental_board.Read Humidity||`` and ``||input:A+B||`` show ``||kitronik_environmental_board.Analog read P1||``.

#### ~ tutorialhint
```blocks
input.onButtonPressed(Button.A, function () {
    basic.showNumber(kitronik_environmental_board.temperature(TemperatureUnitList.C))
})
input.onButtonPressed(Button.AB, function () {
    basic.showNumber(kitronik_environmental_board.readIOPin(kitronik_environmental_board.PinType.analog, kitronik_environmental_board.IOPins.p1))
})
input.onButtonPressed(Button.B, function () {
    basic.showNumber(kitronik_environmental_board.humidity())
})
```

### Step 6
CODING COMPLETE! Click ``|Download|`` and transfer the code to the Environmental Control Board.  
Try varying the conditions to see the sensor values displayed on the LEDs.