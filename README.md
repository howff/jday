# jday

A program for date and time manipulation. The J in the name refers to Julian time.
It can decode dates and times in filenames or given on the command line, or the modification time of a file.
The output can be given in various time formats.
Simple arithmetic can be performed, for example to add or subtract in unit of seconds through weeks.
Note that "jtime" is a julian time in (fractional) minutes.
GHourAngAriesDeg is the Greenwich Hour Angle of Aries, see
https://astronavigationdemystified.com/sidereal-time/

## Usage

```
usage: jday [-d] [-v] [-i|-I] [-t option] [-H] [-s satno] [-A arith] [-R round down arith] [-M file]
-d	debug
-v	verbose
-H	turn off headers, only show values
-i	try to read from stdin
-I	do not try to read from stdin
-s	satellite number, needed to decode MODIS filenames from IPOPP
-t	option shows only the chosen time format(s)
	jday/jtime/ctime/dir/geodir/arcdir/daydir/full/mod/jul/timet/aries/1958/2000/txt
-M	filename modification time
-A	arithmetic: + or - N s=sec/m=min/h=hour/d=day/w=week (eg. +1d)
-R	rounding: as for -A (eg. 15m)
or: YYYY/MM/DD[/hhmm]
or: year month day [HHMM] (HourMinute is optional)
or: year month day HH MM.mm SS.ss mmm
(Minutes, Seconds, milliseconds all optional, may be fractional)
or: HH MM SS mmm DD MM YYYY (same format as in pass txt file)
or: [Mon] Day DD hh:mm:ss[.msec] [TZ] YYYY
or: Day, DD Mon YYYY hh:mm:ss TZ
or: YYYY/MM/DD[/hhmm]
or: YYJJJ (two-digit year and julianday)
or: YYYYJJJHHMMSSMS (year, julian day, time, milliseconds)
or: year julianday
or: year julianday millisecondsofday
or: julianday (may be fractional too) assumes current year
or: jtime  	  (minutes from 1900)
or: time_t 	  (seconds from 1970)
or: secs_since_1958 nnn (seconds from 1958)
or: secs_since_2000 nnn (seconds from 2000)
or: filename.PDS (eg. P?????????????????????YYJJJHHMMSS???.PDS)
or: a1.YYJJJ.HHMM or t1.YYJJJ.HHMM (MODIS DB style)
or: PYYYYJJJHHMMSS.PDS (Plymouth-style)
or: *YYYYJJJHHMMSS.L1A (SeaWiFS-L1A)
or: AVHR_xxx_M02_YYYYMMDDHHMMSS_* (EPS GenericProductFormat)
or: RNSCA-RVIRS_npp_dYYYYMMDD_tHHMMSSS_*.h5 (NPP)
or: O2_ddMONyyyy_pth_row_* (Oceansat-2)
or: .auto-TIMET (.zfs/snapshot)
```

## Examples

```
% jday -M myfile
julian_day 023
jtime 63146254.86666666
jpr_ctime Thu Jan 23 13:34:52.000 2020
dir 2020/1/23/1334
geodir 2020/1/23/1334
arcdir 2020/1/23/1334
daydir 2020/01/23
full_julian 2458872.06587963
modified_julian 58871.56587963
jday_julian 2458872.06587963
time_t 1579786492
GHourAngAriesDeg 326.08048749
secs_since_1958 1958477691.99999952
secs_since_2000 633101691.99999952
txt_format 13 34 52 000 23 01 2020
geosat ?
channel -1
```

Display the file modication time in seconds since the epoch:

```
% jday -M myfile -t timet
time_t 1579786492
```

As above but without a header/prefix, useful in a script:

```
% jday -M myfile -t timet -H
1579786492
```

As above with arithmetic to subtract 2 days:

```
% jday -M myfile -t timet -H -A -2d
1579613692
```

Test if a file has been modified within the last two days:

```
if [ $(jday -H -t timet -M myfile -A +2d) -gt $(jday -H -t timet) ]; then
```
```
