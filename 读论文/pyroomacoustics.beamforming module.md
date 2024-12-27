## pyroomacoustics.beamforming module

- fs （int） – 采样频率N（int，可选）– FFT 的长度，即 FD 波束成形权重的数量，等距。 默认值为 1024。
- Lg （int， 可选） - 时域滤波器的长度。默认值为 N。
- hop （int， 可选） - 用于频域处理的跳数长度。默认值为 N/2。
- zpf （int， 可选） – 用于频域处理的前零填充长度。默认值为 0。
- zpb （int， 可选） – 用于频域处理的零填充长度。默认值为 0。