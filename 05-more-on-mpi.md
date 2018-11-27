# More on MPI

**Elster's Bit-Reversal algorithm**\
Fast Fourier Transform (FFT) has an important role in performing parallel simulations such as plasma simulation, weather forecasting, and dynamic fluids.  Bit-reversal routine is considered to be an essential part of FFT and that is because of high possibility of degrading the overall execution time of FFT application if it is not perfectly designed.

The formulation is based on independent steps, hence it can be efficiently implemented on parallel computers.

The naive approach to bit reversal, such as the one used by the FFT algorithm is O(NlogN), for generating the bit-reversed sequence of N numbers. Elster’s Bit-Reversal algorithm, however, is O (N)