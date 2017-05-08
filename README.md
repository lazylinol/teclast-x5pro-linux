# Teclast X5 Pro hardware support for Linux

## Touchscreen

### Story
Initially I was able to get touchscreen working with https://github.com/onitake/gslx680-acpi driver, but cursor was jumping around small area near pointed finger. Research showed that it is possible to add some fuzz to coodinates detection, so small noise in X and Y parts are ignored.
Modified driver can be found [here](https://github.com/LazyLinol/gslx680-acpi)

### Firmware
Touchscreen driver need proper firmware to be available.
Firmware was extracted from Windows 10 Silead [driver](https://github.com/LazyLinol/teclast-x5pro-linux/tree/master/touch/win_driver) following the instructions from https://github.com/onitake/gsl-firmware and also [this one](https://github.com/onitake/gsl-firmware/blob/master/firmware/trekstor/surftab-twin-10.1-ST10432-8/README.md#command-to-find-the-offsets-used-for-extraction).

mssl1680.fw is received as result, and then converted to the format suitable for gslx680-ts-acpi driver:
```
./fwtool -c mssl1680.fw -m 1680 -w 1980 -h 1530 -t 1 -f track silead_ts.fw
```
Firmware must be put into `/lib/firmware` and named `silead_ts.fw`.

### Mainstream driver
It is also possible to use silead.ko driver from Linux kernel. It still requires firmware to work properly. This one needs to be unconverted, so directly extracted file (mssl1680.fw) from Win driver is okay.
It has to be put into `/lib/firmware/silead` and named `mssl1680.fw`.
