# aqi

Fork of the [sensor project from the official joy-it documentation](https://github.com/zefanja/aqi).

Original project was created to:
> Measure AQI based on PM2.5 or PM10 with a Raspberry Pi and a SDS011 particle sensor

This fork is created to enable usage of the python3 on my laptop running
Ubuntu 20.04.1.

## how to install python3 libraries

Required python3 libraries can be install using the following command:
`sudo apt install git-core python3-serial`

## how to enable starting aqi as a non-root user

By default, `/dev/ttyUSB0` can'b be read by the non-root user. To enable reading
it as my laptop user I had to add this user to the `dialout` group:
`usermod -a -G dialout $USER`

After that you should loggout and login to apply changes but as a work-around
you can run the following code:
```
original_group="$(id -gn)"
newgrp dialout
newgrp $original_group
```

## how to install lighttpd

Run the:
`sudo apt install lighttpd`

## how to run sensor

From the cloned `aqi` repo from the root directory run:
`python3 python/aqi.py`

Program will output something like:
```
Y: 0, M: 177, D: 0, ID: 0xbc7f, CRC=OK
PM2.5:  3.0 , PM10:  18.2
PM2.5:  3.0 , PM10:  18.7
PM2.5:  3.1 , PM10:  19.6
PM2.5:  3.1 , PM10:  19.8
PM2.5:  3.1 , PM10:  19.2
PM2.5:  2.9 , PM10:  19.2
PM2.5:  2.8 , PM10:  19.8
PM2.5:  2.8 , PM10:  19.4
PM2.5:  2.8 , PM10:  19.2
PM2.5:  2.8 , PM10:  19.1
PM2.5:  2.8 , PM10:  18.7
PM2.5:  2.7 , PM10:  18.6
PM2.5:  2.7 , PM10:  18.9
PM2.5:  2.7 , PM10:  19.1
PM2.5:  2.7 , PM10:  18.0
Going to sleep for 1 min...
PM2.5:  2.9 , PM10:  18.4
PM2.5:  2.9 , PM10:  18.6
...
```

The file `html/aqi.json` will contain JSON with the sensor information for the
last measurement in each set of measurments (approx. each 1.5 min):
```
[
  {
    "pm25": 2.2,
    "pm10": 9.8,
    "time": "22.01.2021 17:13:00"
  },
  {
    "pm25": 3,
    "pm10": 22,
    "time": "22.01.2021 17:14:30"
  },
  ...
]
```

## how to run lighttpd server

- copy the template file `lighttpd.config.template` to the configuration
  `lighttpd.config` file
- edit the first line with proper absolute path to the `html` directory of this
  project
- run: `lighttpd -D -f lighttpd.config`

Web page with the current sensor results is available at:
- http://localhost:3000/
