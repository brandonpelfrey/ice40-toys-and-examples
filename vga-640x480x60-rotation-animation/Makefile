# Project setup
PROJ      = vga-rotation
BUILD     = ./build
DEVICE    = 8k
FOOTPRINT = ct256
YOSYS_OPTS= -noautowire # Generate errors if wires were implicitly created

# Files
FILES = top.v

.PHONY: all clean burn

all:
	# if build folder doesn't exist, create it
	mkdir -p $(BUILD)
	# synthesize using Yosys
	yosys -q -p "read_verilog -noautowire top.v" -p "synth_ice40 -top top -blif $(BUILD)/$(PROJ).blif"
	# Place and route using arachne
	arachne-pnr -d $(DEVICE) -P $(FOOTPRINT) -o $(BUILD)/$(PROJ).asc -p pinmap.pcf $(BUILD)/$(PROJ).blif
	# Convert to bitstream using IcePack
	icepack $(BUILD)/$(PROJ).asc $(BUILD)/$(PROJ).bin

burn: all
	iceprog -S $(BUILD)/$(PROJ).bin

clean:
	rm build/*
