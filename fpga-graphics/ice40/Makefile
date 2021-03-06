## Project F: FPGA Graphics - iCEBreaker Makefile
## (C)2020 Will Green, open source hardware released under the MIT License
## Learn more at https://projectf.io

# configuration
SHELL = /bin/sh
FPGA_PKG = sg48
FPGA_TYPE = up5k
PCF = icebreaker.pcf

# included modules
PATH_CM = ../../common
ADD_SRC = ${PATH_CM}/display_timings.sv ${PATH_CM}/ice40/clock_gen.sv

top_bounce: top_bounce.rpt top_bounce.bin
top_beam: top_beam.rpt top_beam.bin
top_square: top_square.rpt top_square.bin

%.json: %.sv
	yosys -ql $(subst .json,,$@)-yosys.log -p 'synth_ice40 -top $(subst .json,,$@) -json $@' $< $(ADD_SRC)

%.asc: %.json
	nextpnr-ice40 --${FPGA_TYPE} --package ${FPGA_PKG} --json $< --pcf ${PCF} --asc $@

%.rpt: %.asc
	icetime -d ${FPGA_TYPE} -mtr $@ $<

%.bin: %.asc
	icepack $< $@

clean:
	rm -f top*.json top*.asc top*.rpt top*.bin top*yosys.log

all: top_bounce top_beam top_square

.PHONY: all clean
