import pyoscilloscope as posc
import usbtmc
import matplotlib.pyplot as plt
import numpy as np
import time

class Infiniivision:
    def __init__(self, pos = 0):
        """Initializes a Keysight InfiniiVision oscilloscpe.

        Args:
        pos -- position of device in devices list (provided by list_devices() from PyOscilloscope) (default 0)
        """
        self.inst = posc.instrument(pos)
        self.gen = posc.Generator(self.inst)
        self.chan = posc.Channels(self.inst)
        self.osc = posc.Oscilloscope(self.inst)    
    
    def get_freq(self, channel=1):
       return self.chan.get_frequency(channel)	

    def get_vpp_(self, channel=1):
        return self.chan.get_vpp(channel)
    
    def show_img(self, channel=1):
        """Prints acquired waveform from selected channel on PC.

        Args:
        channel -- selected channel (default 1)
        """
        self.osc.stop()
        time, data = self.chan.get_data(channel)
        self.osc.run()
        
        if (time[-1] < 1e-3):
            time = time * 1e6
            t_unit = "uS"
        elif (time[-1] < 1):
            time = time * 1e3
            t_unit = "mS"
        else:
            t_unit = "S"
        
        voltscale = self.chan.get_volt_scale(channel)
        voltoffs = self.chan.get_volt_offset(channel)
        
        plt.plot(time, data)
        plt.title("Oscilloscope Channel {}".format(channel))
        plt.ylabel("Voltage (V)")
        plt.xlabel("Time ({})".format(t_unit))
        plt.xlim(time[0], time[-1])
        plt.ylim(-4*voltscale, 4*voltscale)
        plt.show()

    def time_constant(self, channel=1):
        self.osc.stop()
        time, data = self.chan.get_data(channel)
        #return the time constant tau
        return time[data.index(0.632*max(data))]     
    
    def get_phase_(self, sel_channel=1, ref_channel=2):
        return self.chan.get_phase(sel_channel, ref_channel)

    def gen_sin_wave(self, amp, freq, offs):
        self.gen.sin(amp, freq, offs)
        return freq

    def gen_sqr_wave(self, amp, freq, duty_cicle=50.0,offs=0.0):
        self.gen.sqr(amp, freq, duty_cicle, offs)
        return freq
    
    def set_time_scale(self, scale):
        self.chan.set_time_scale(self, scale)

    def capacitance(self, freq, resistor):
        self.gen_sin_wave(0.5, freq, 0)
        self.set_time_scale(0.4//f)
        phase = self.get_phase_()
        freq = self.get_freq()
        c = np.tan(phase*2*np.pi/360)/(2*np.pi*freq*resistor)
        
        return c, freq


    def capacitance_sqr(self, freq, resistor)
        self.gen_sqr_wave(0.5, freq)
        self.set_time_scale(0.4//f)
        tau = self.time_constant()
        c = tau/resistor
        return c

    def multiple_waves(self, resistor):
        capac_list = []
        for i in range(100, 10000, 1000):
            #can be capacitante or capacitante_sqr:
            capac_list.append(self.capacitance(i, resistor))
            time.sleep(0.5)
        plt.plot([x[0] for x in capac_list], [x[1] for x in capac_list])
        plt.show()
        return 

test = Infiniivision()
#freq = test.get_freq()
#print(test.get_vpp_())
#phase = test.get_phase_()
print(test.capacitance(1000, 10000))

#print(test.show_img())
