---
title: "REDHAWK Core Assets"
weight: 10
---

REDHAWK packages Core Assets that consist of starter/example Components, Devices, Waveforms, and Shared Libraries.

### REDHAWK Basic Components

REDHAWK Basic components are a collection of starter/example REDHAWK components. These components provide basic DSP capabilities while demonstrating many of the features and use cases of REDHAWK.

{{% notice note %}}
The following Basic components have been removed:

  - `DataWriter` and `DataReader` have been deprecated by `FileWriter` and `FileReader`, respectively.
  - `whitenoise` has been deprecated by `SigGen`, which includes a whitenoise generation mode.
  - `BurstDeserializer`, `freqfilter`, `medianfilter`, and `unwrap` have been removed.

The following Basic components have been updated to use the new shared library dependencies: `agc`, `AmFmPmBasebandDemod`, `ArbitraryRateResampler`, `autocorrelate`, `fastfilter`, `psd`, and `TuneFilterDecimate`.
{{% /notice %}}

| **component**               | **Version** | **Description** |
| :-------------------------- | :---------: | :------- |
| `rh.agc`                    |     2.0     | Provides automatic gain control to normalize power levels for real or complex signals.|
| `rh.AmFmPmBasebandDemod`    |     2.0     | Performs analog demodulation on complex baseband input signals. |
| `rh.ArbitraryRateResampler` |     2.0     | Resamples a data stream at output rates that are not limited to integer multiples of the input sampling rate. This component can increase or decrease the sample rate. No anti-aliasing filtering is included, therefore, you must use this component with caution when decreasing the sampling rate to avoid aliasing or, if required, pre-filter in an upstream component. |
| `rh.autocorrelate`          |     2.0     | Performs a frequency domain implemenation of a windowed autocorrelation algorithm. This algorithm works by windowing the input data to break it up into separate frames. Each frame is independently autocorrelated with each other frame using a "full" autocorrelation, which includes the full transient response. This is efficiently computed in the frequency domain. |
| `rh.DataConverter`          |     4.0     | Converts between BulkIO data types in REDHAWK. With proper configuration, converts between any of the following data types; Char, Octet, Unsigned Short, Short, Float, and Double. Also capable of converting real data into complex data, and similarly, complex data into real data. By default, nothing needs to be configured. When using float or double output BulkIO ports, set the `normalize_floating_point` with the `floatingPointRange` of -1 and 1.  |
| `rh.fastfilter`             |     2.0     | Provides a FIR filter implementation using the FFT-based overlap-add technique. This component includes a filter designer, which aids in impulse response creation for lowpass, highpass, bandpass, and bandstop filters. The component accepts custom impulse responses via property configuration.  |
| `rh.fcalc`                  |     2.0     | Enables users to perform calculations on one or two input streams simultaneously on an element by element basis. The calculation is specified in Python syntax using `a` and `b` to denote input on the a and b ports. Valid examples: `3*a+b` and `a*math.cos(b)`.  |
| `rh.FileReader`             |     4.0     | Reads in a file and converts it to BulkIO data to be streamed out. Capable of sending data, regardless of file format, out of most of its ten data ports. This ability excludes the XML port. To use this component, the `file_format` must match that of the file and you are required to specify a `source_uri` (location of the file); a `sample_rate`; the `center_frequency` when applicable; and the playback state to play. |
| `rh.FileWriter`             |     4.0     | Writes out a data file from a BulkIO stream. It is not capable of changing the file format to be different from the BulkIO input stream. To use this component, you are required to specify a `destinations_uri` (location to write to) and a file format and set `recording_enable` to `true`.  |
| `rh.HardLimit`              |     2.0     | Thresholds data so that all data is between the upper and lower limit as specified by the properties. |
| `rh.psd`                    |     2.0     | Transforms data from the time domain to the frequency domain using an FFT-based PSD. Output data is framed data where each frame contains the frequency domain representation of a subsection of the input. This component provides both the real-valued PSD and the complex FFT outputs. |
| `rh.psk_soft`               |     2.0     | Takes complex baseband pre-d data and does a PSK demodulation of either BPSK, QPSK, or 8-PSK and outputs symbols and bits. Input must be an integer number of samples per symbol (recommended 8-10). |
| `rh.RBDSDecoder`            |     2.0     | Decodes RBDS data from broadcast FM using the RBDS Standard Specification. |
| `rh.SigGen`                 |     2.0     | Generates different output signals based on its configuration. Contains an implementation in each of the supported languages (Python, C++, Java) and is an example of a component with multiple implementations. |
| `rh.SinkSDDS`               |     1.0     | Accepts BulkIO data and produces a single SDDS stream over the provided multicast or unicast address. |
| `rh.sinksocket`             |     2.0     | Reads data from a BulkIO port and writes it to a TCP socket.  |
| `rh.SinkVITA49`             |     3.0     | Creates a UDP/multicast or TCP VITA49 packet stream and converts the data and SRI Keywords to IF data packets and Context packets for use within/between/outside of a REDHAWK domain application.  |
| `rh.SourceSDDS`             |     1.0     | Consumes a single SDDS formatted multicast or unicast UDP stream and outputs the data via the appropriate BulkIO native type.  |
| `rh.sourcesocket`           |     2.0     | Reads data from a TCP socket and writes it to a BulkIO port. |
| `rh.SourceVITA49`           |     3.0     | Connects to a UDP/multicast or TCP VITA49 packet stream and converts the headers to SRI Keywords and data to the BULKIO interface of the user’s choice for use within REDHAWK domain applications. |
| `rh.TuneFilterDecimate`     |     2.0     | Selects a narrowband cut from an input signal. Tuning, filtering, and decimation are used to remove noise and interference in other frequency bands and reduce the sampling rate for more efficient downstream processing.  |

### REDHAWK Basic Devices

| **Device**          | **Version** | **Description**   |
| :------------------ | :---------: | :------- |
| `rh.FmRdsSimulator` |     2.0     | Designed to be used in conjunction with the `libRfSimulators` library. Using the simulator library, this FrontEnd Interfaces compliant REDHAWK device will generate FM modulated mono or stereo audio with RDS encoded PI (Call Sign), PS (Short Text), and RT (Full Text) data. |
| `rh.MSDD`           |     3.0     | FrontEnd Interfaces compliant device for the MSDD-X000 series receivers. Supports multiple FPGA loads for the target hardware. The device provides an RX_DIGITIZER and an RX_DIGITIZER_CHANNELIZER capability.  |
| `rh.RTL2832U`       |     2.0     | Interfaces with the Realtek RTL2832U usb dongle device using the `librtlsdr` Device dependency. Supports various tuners, including Elonics E4000, Rafael Micro R820T and R828D, Fitipower FC0012 and FC0013, and FCI FC2580.   |
| `rh.USRP_UHD`       |     4.0     | FrontEnd Interfaces compliant device for the USRP that requires the UHD host code and supporting libraries to be installed. |

### REDHAWK Basic Waveforms

| **Waveform**                  | **Version** | **Description**  |
| :---------------------------- | :---------: | :---------- |
| `rh.basic_components_demo`    |     1.0     | Uses a few core assets. Data from `SigGen` is created and manipulated to demonstrate REDHAWK.  |
| `rh.FM_RBDS_demo`             |     1.0     | Processes the RBDS digitial message present in broadcast FM radio signals. Tunes and filters out this signal, performs a BPSK demodulation, and then decodes the RBDS data.  |
| `rh.FM_mono_demo`             |     1.0     | Processes the mono audio channel of broadcast FM Radio.  |
| `rh.short_file_to_float_file` |     1.0     | Reads in a file containing shorts, converts them to floats in REDHAWK, and then writes out the file. Uses three components: `FileReader`, `DataConverter`, and `FileWriter`.  |
| `rh.socket_loopback_demo`     |     1.0     | Puts REDHAWK data out onto a network socket using sinksocket and then reads it back from the socket into REDHAWK via sourcesocket.  |
| `rh.VITA49_loopback_demo`     |     1.0     | Uses the `sourceVITA49` and `sinkVITA49` components to demonstrate how VITA49 network data can move into and out of REDHAWK.  |

### REDHAWK Shared Libraries

{{% notice note %}}
All shared library (formerly soft package) dependencies have been repackaged using the new code generators, and the following basic components have been updated to use the new shared library dependencies: `agc`, `AmFmPmBasebandDemod`, `ArbitraryRateResampler`, `autocorrelate`, `fastfilter`, `psd`, and `TuneFilterDecimate`.

The following shared libraries have been renamed:

  - `redhawk-VITA49Libraries_V1` has been renamed `VITA49`.
  - `RedhawkDevUtils_v1` has been renamed `RedhawkDevUtils`.
{{% /notice %}}

| **Shared Library**            | **Version** | **Description**  |
| :---------------------------- | :---------: | :---------- |
| `rh.blueFileLib`    |     2.0     | Provides a library for interacting with Midas BLUE files, the standard binary file format used by modern Midas frameworks.  |
| `rh.dsp`             |     2.0     | Provides a DSP library for basic components. Much of the actual signal processing code is contained in this library, which is independent of REDHAWK.  |
| `rh.fftlib`             |     2.0     |Provides a DSP library wrapping FFTW for basic components. Provides a higher level of functionality than what is contained in FFTW by providing classes for taking FFTs and their inverses and signal processing classes, which leverage these FFT capabilities.  |
| `rh.RedhawkDevUtils` |     4.0     | Provides a library that contains utility functions useful for REDHAWK development. Utility functions are included for working with vectors, byte swapping, file I/O, universal data types, and transforms between data types, as well as assisting with compatability across multiple `boost` versions. Additionally, various REDHAWK helper functions are provided for UUID generation and CORBA serialization, and working with Properties, BulkIO, base-64 characters, strings, and Domain Managers.  |
| `rh.VITA49`     |     3.0     | Provides VITA Radio Transport (VRT) libraries that represent a (nearly-complete) software-based implementation of the following ANSI specifications: <ul><li>VRT/VITA 49.0</li> <li>VITA Radio Link Layer (VRL) / VITA 49.1</li></ul>  |

### REDHAWK Device Dependencies

| **Device Dependency** | **Version** | **Description**   |
| :-------------------- | :---------: | :-------- |
| `libRfSimulators`     |     1.1     | Used to simulate an RF Digitizer. At present, the RF Simulators library contains a single implementation: the FM RDS Simulator. While the RF Simulators library was designed to be used within the REDHAWK SDR environment, the library itself has no dependency on the REDHAWK framework and may be used as a stand alone C++ library. |
| `librtlsdr`           |    0.5.2    | Provides the rtl-sdr hardware driver that enables Realtek RTL2832-based DVB dongles to be used as SDR receivers. Managed by osmocom.org.  |
| `uhd`                 |    3.7.3    | Provides the USRP hardware driver (UHD) for use with the USRP product family. Managed by ettus.com. This is the version shipped with REDHAWK; however, some USRP hardware may require a more current version.  |
