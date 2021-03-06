.. _refEDay2:

***********************************
Exercises Day 2
***********************************

.. |Ve| replace:: V\ :sub:`e`\
.. |Ce| replace:: C\ :sub:`e`\
.. |Rm| replace:: R\ :sub:`m`\
.. |Re| replace:: R\ :sub:`e`\
.. |Cs| replace:: C\ :sub:`s`\
.. |Vin| replace:: V\ :sub:`in`\
.. |Vec| replace:: V\ :sub:`ec`\
.. |Vout| replace:: V\ :sub:`out`\

.. contents::
  :depth: 2
  :local:

1. Capacitor and Resistor in parallel
#########################################

In the circuit below, you will see a capacitor and a resistor in parallel. The voltage source alternates at 20Hz, going from -10 to +10 volts. The current travels over the resistor or via the capacitor to ground.

.. image:: ../_static/images/EEA/eea_fig-74.png
  :align: center
  :target: https://tinyurl.com/yhu578fx

.. container:: exercise

  1A.  Increase the value of the resistor to 200kOhm. What happens to the current?

  1B.  Put the resistor back to 1kOhm. Now, increase the capacitance of the capacitor to around 10mF. What happens to the current?

  1C.  Return the values to 1kOhm and 10uF.

  Change the frequency of the alternating signal to:
  - 1000 Hz (action potentials!)
  - 1 Hz
  You may have to change the simulation speed using the red slider at the top right, and adjust the x-scaling of the scope below (right-click / properties / and slide the 'horizontal scale').

  1D. How does the frequency of the signal relate to how much current crosses either the capacitor or the resistor?

2. The equivalent circuit of the electrode
##############################################

In the theory handout, we discussed how we can represent an electrode as a circuit containing a resistance and a capacitance. We’ll now build this equivalent circuit in the simulator.

.. container:: exercise

    2A.	Edit the circuit used above to build the equivalent circuit of a polarised, tungsten electrode.

    Here are some values to use:

    *	|Rm|: the DC resistance of the metal electrode wire, 10-100 Ohms.
    *	|Ce|: the electrode tip capacitance, generated by the double layer generated around the electrode.  |Ce| ~ 0.2 pF / µm2, so 10 - 20 pF (if the electrode is unplated)
    *	|Re|: electrode tip resistance, in parallel with |Ce|. ~ 100 MOhm.

    2B.	Edit the alternating voltage supply to provide 1V at 1Khz, mimicking the signal coming from your cell. 1V is larger than ephys signals, but makes the current flow easier to see. Change the sliders for simulation speed and current speed until you can see where the current is flowing.

    2C. What happens if you delete the resistor |Re|?

    2D. Bonus exercise: Can you change this circuit from a polarising, tungsten electrode, to a circuit representing a nonpolarizable electrode?

3. Impedance
##########################

Here is the circuit, with a shunt impedance added of 100 pF:

.. image:: ../_static/images/EEA/day2circuit.png
  :align: center
  :target: https://tinyurl.com/y2jshzqc

.. container:: exercise

  3A. What is shunt impedance?

  3B. What % of our signal are we losing between Vec and Vin? Why?

  3C. What could we change to increase the signal at Vin?

  3D. The tips of certain electrodes, e.g. nichrome tetrodes, can be electroplated in a thin layer of gold. This 'goldplating' increases the surface area of the tip, creating more space to separate charge. This increases tip capacitance by around 100x. Make this change in the circuit. How similar are Vec and Vin now?

Drawing Current
***********************************
Let's do a recording! We'll take our electrode, and attach it through a long wire to a recording system (the recording system has an analog to digital converter, ADC, and a recording computer). The leakage resistance here is because the recording system is also connected to ground.

.. image:: ../_static/images/EEA/day2withac.png
  :align: center
  :target: https://tinyurl.com/y6864vle

.. container:: exercise

  3E. How much of the voltage at the electrode, Vec, are we recording at Vout?

  3F. Place an ideal operational amplifier between the electrode and the long wire. What happens to Vout? Why?

  3G. Change the circuit to stop the amplifier from saturating. What is the amplifier gain now?

4. Operational Amplifiers
###################################
Let’s build a slightly simplified version of these circuits in real life, on the breadboard. We’ll treat the ‘Blink’ example as our neuronal data and see what happens to this signal if we just have a wire, and then see the effect of replacing this wire with an op-amp. We'll simplify our electrode circuit to a single resistor.


* 'Neuron'  = Digital blink output from Teensy
* 'Electrode' = 100 kOhm resistor
* 'Shunt' = 22kOhm resistor
* 'Leak' = 220Ohm resistor
* 'Recording system' = the Picoscope


.. image:: ../_static/images/EEA/circuitday2.png
  :align: center
  :target: https://tinyurl.com/yyeah3wd

Without an amplifier
************************************

.. container:: exercise

  4A.	Upload the Blink example to your teensy (or just run it if still loaded).

  Build the circuit below:

  * Send the Teensy output through a 100 KOhm resistor. This makes it behave a bit like a biological signal coming from an electrode.

  *	A 22kOhm resistor to ground simulates shunt impedance.

  * A 220 Ohm resistor to ground simulates that your acquisition system is connected to ground (via some resistance).

  *	The yellow wires are 'readout' wires to connect your oscilloscope to.

  .. image:: ../_static/images/EEA/eea_fig-39.png
    :align: center

  .. image:: ../_static/images/EEA/eea_fig-38.png
    :align: center

  4B.	Now measure the output with the oscilloscope at the points marked by red arrows in the image below, and complete the first column of the table below:

  .. list-table::
     :width: 80%
     :widths: 20 20 20
     :header-rows: 1
     :align: left

     * - (+) Probe Location
       - Long Wire
       - Op-Amp
     * - 1. Teensy Pin 13
       -
       -
     * - 2. Readout Wire 1
       -
       -
     * - 3. Readout Wire 2
       -
       -

  4C. How much signal is lost?

Build voltage rails
***********************************
.. warning::
  Make sure that the pins from the batteries do not touch, and if they’re not in use, best to put some tape on them so they don’t touch things. ‘Short-circuiting’ the batteries (connecting them without any sort of resistance) causes a huge current to flow from the + to -, enough to... melt stuff.

Now, we need to make the ‘rails’ that will provide the voltage for our op-amp. Eventually, for our EMG circuit, we will need to have a positive and negative voltage ready, so that we can amplify a signal that lives around some reference level that we shall call 0 volt. If we only have 0 and +3V, then any negative signal will floor and stay at 0.

To do this we use a common trick and turn two regular power supplies into a bipolar power supply. In our case we use batteries, because they’re cheap and pretty much fully noise-free. Check which way up your breadboard is (keep the blue line at the top). Following the figures precisely will make debugging much easier later on.

.. container:: exercise

  4D. Connect the battery holders as follows:

  - One pair of batteries provides 3V relative to ground, 0V.

  - Both ground rails are connected through a wire.

  - The second pair of batteries is reversed to provide -3V relative to ground, so that we get a + and a – voltage.

  - Remember or label which side is +3 and which is -3

  .. image:: ../_static/images/EEA/eea_fig-35.png
    :align: center

Add bypass capacitors
***********************************
Bypass capacitors are small capacitors that act like little secondary batteries. In our case we’ll add two 100nF (marked 104) caps, one to each rail, so GND to 3V and GND to -3V. The reason is that the batteries we use have what's called a high ESR - ‘equivalent series resistance’ and some capacitance, so they are not great at quickly providing current. This means that when our op-amp starts working, it can run out of current for a very short time, until the battery can drive the rails back to their original voltage. This is bad for the signal quality.
So, we give the rails the ability to very quickly provide a small amount of current from these small capacitors. We’re exploiting the fact that these caps have very low ESR and can provide current pretty much instantaneously. If the battery briefly can’t provide current, the bypass capacitors will discharge, providing quick back-up current. The fact that they’re too small to power anything for more than a millisecond does not matter here, at that point the batteries have caught up.

.. container:: exercise

  4E. Add two 100nF (marked 104) caps, one to each rail, so connecting GND to 3V and connecting GND to -3V (see image below).

  .. image:: ../_static/images/EEA/eea_fig-36.png
    :align: center

Replace the 'long wire' with 'headstage'
***********************************************
We will replace our long wire with a 'headstage'. We will use only the most basic part of the headstage, an operational amplifier.

This is the op-amp you have.  Make sure you’re looking at the op-amp (AS358P), not the instrumentation amp.

.. image:: ../_static/images/EEA/eea_fig-41.png
  :align: center

.. container:: exercise

  4F. Add the op-amp to the circuit.

  * Place the op-amp on your breadboard, with the semicircle cutout on the left.

  * Connect the +3 voltage rail to ‘Vcc+’ and the -3 voltage rail to ‘Vcc-‘

  * Put the electrode output wire into the + input of your op-amp, and the output of the op-amp into the ‘wire’ simulation circuit.

  * Feed the output of the op-amp, back into the – input.

  .. image:: ../_static/images/EEA/eea_fig-42.png
    :align: center

  .. image:: ../_static/images/EEA/eea_fig-40.png
    :align: center

  4G. Now measure the same three points as before and complete the table in question 4B.         -

  4H. Optional: try changing the resistances you've used for electrode, shunt, and leakage. What happens to the signal?
