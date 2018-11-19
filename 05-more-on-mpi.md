# More on MPI

**Elster's Bit-Reversal algorithm**\
Fast Fourier Transform has an important role in performing parallel simulations such as plasma simulation, weather forecasting, and dynamic fluids.  Bit-reversal routine is considered to be an essential part of FFT and that is because of high possibility of degrading the overall execution time of FFT application if it is not perfectly designed.

The formulation is based on independent steps; hence it can be efficiently implemented on parallel computers.

Elster’s Bit-Reversal algorithm is O (N)