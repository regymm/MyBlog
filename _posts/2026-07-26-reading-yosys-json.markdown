---
layout: post
title: "Reading JSON generated from Yosys (Xilinx ver.)"
data: 2025-07-26 22:30:00 +800
comments: true
categories: [Experience]
---

Have you been using the open-source toolchains for FPGAs ([OpenXC7](https://github.com/openXC7), [PrjTrellis](https://github.com/YosysHQ/prjtrellis), [IceStorm](https://github.com/YosysHQ/icestorm), etc.)? Yosys is a critical part to generate the Verilog RTL code into netlists. Just like the sick hobby of reading and writing assembly, it’s a great accomplishment to directly read or even write the JSON netlist, and convert it into bitstreams!

But netlists are sometimes millions of lines long and, of course, hard to read: a Xilinx one for a simple design may give over 5 MB!

This is the simplest design I can think of: two assignments and even less than a blinky!

```verilog
`timescale 1ns / 1ps
 
module top(
    input clk,
	output [1:0]o
    );
    assign o[0] = clk;
    assign o[1] = clk;
endmodule
```

The JSON file compiled by `yosys -p "synth_xilinx -flatten -abc9 -arch xc7 -top top; write_json build/top.json"`, the standard OpenXC7 flow, has 232824 lines ([Download](/MyBlog/images/readingyosysjson.zip)). 

This is daunting to start with, but the truth is, only the last segment contains the top module. The others are just having the whole `stdio.h` content inside user source code: in the `modules` entry,

It’s looking at this:

```json
    "top": {
      "attributes": {
        "hdlname": "top",
        "top": "00000000000000000000000000000001",
        "src": "top.v:4.1-10.10"
      },
      "ports": {
        "clk": {
          "direction": "input",
          "bits": [ 2 ]
        },
        "o": {
          "direction": "output",
          "bits": [ 3, 4 ]
        }
      },
      "cells": {
        "$iopadmap$top.clk": {
          "hide_name": 1,
          "type": "IBUF",
          "parameters": {
          },
          "attributes": {
            "keep": "00000000000000000000000000000001"
          },
          "port_directions": {
            "I": "input",
            "O": "output"
          },
          "connections": {
            "I": [ 2 ],
            "O": [ 5 ]
          }
        },
        "$iopadmap$top.o": {
          "hide_name": 1,
          "type": "OBUF",
          "parameters": {
          },
          "attributes": {
            "keep": "00000000000000000000000000000001"
          },
          "port_directions": {
            "I": "input",
            "O": "output"
          },
          "connections": {
            "I": [ 5 ],
            "O": [ 3 ]
          }
        },
        "$iopadmap$top.o_1": {
          "hide_name": 1,
          "type": "OBUF",
          "parameters": {
          },
          "attributes": {
            "keep": "00000000000000000000000000000001"
          },
          "port_directions": {
            "I": "input",
            "O": "output"
          },
          "connections": {
            "I": [ 5 ],
            "O": [ 4 ]
          }
        }
      },
      "netnames": {
        "$abc$1682$iopadmap$clk": {
          "hide_name": 1,
          "bits": [ 5 ],
          "attributes": {
          }
        },
        "clk": {
          "hide_name": 0,
          "bits": [ 2 ],
          "attributes": {
            "src": "top.v:5.11-5.14"
          }
        },
        "o": {
          "hide_name": 0,
          "bits": [ 3, 4 ],
          "attributes": {
            "src": "top.v:6.14-6.15"
          }
        }
      }
    }
```

This is much clearer to read: 

- Attribute specifies the top module
- Ports contain the the I/O pins of the top design, `clk` and `o[1:0]`, each assigned a signal number (2, 3, 4)
- Cells are the primitives used, it’s not visible in the source code here, but the IBUF and OBUF will be inserted to each I/O pin. Parameters of the IBUF/OBUF are not used here. And the connections to the ports are shown by the same signal number: 2, 3, 4 are used, and a new intermediate wire, 5, is also implied.
- Netnames gives each wire a name, making the log for later stages easier to read. It’s like the “debug info” for ELF files. For implied wires not defined in the source code, automatic names are used and attached to them.

The wiring can be visualized like this:

![](/MyBlog/images/combinatorial.png)

Well, then what’s inside the “header file”-like 20,0000+ lines above?

It seems every primitive on every Xilinx device is here, from the simple MMCM, OBUF, IBUF, to complex PCIE_2_1, to GTYE3_CHANNEL that doesn’t exist on devices supported by OpenXC7 yet.

For example, the OBUF “header” is:

```json
    "OBUF": {
      "attributes": {
        "blackbox": "00000000000000000000000000000001",
        "src": "/usr/local/bin/../share/yosys/xilinx/cells_sim.v:59.1-71.10"
      },
      "parameter_default_values": {
        "CAPACITANCE": "DONT_CARE",
        "DRIVE": "00000000000000000000000000001100",
        "IOSTANDARD": "DEFAULT",
        "SLEW": "SLOW"
      },
      "ports": {
        "O": {
          "direction": "output",
          "bits": [ 2 ]
        },
        "I": {
          "direction": "input",
          "bits": [ 3 ]
        }
      },
      "cells": {
        "$specify$2": {
          "hide_name": 1,
          "type": "$specify2",
          "parameters": {
            "DST_WIDTH": "00000000000000000000000000000001",
            "FULL": "0",
            "SRC_DST_PEN": "0",
            "SRC_DST_POL": "0",
            "SRC_WIDTH": "00000000000000000000000000000001",
            "T_FALL_MAX": "00000000000000000000000000000000",
            "T_FALL_MIN": "00000000000000000000000000000000",
            "T_FALL_TYP": "00000000000000000000000000000000",
            "T_RISE_MAX": "00000000000000000000000000000000",
            "T_RISE_MIN": "00000000000000000000000000000000",
            "T_RISE_TYP": "00000000000000000000000000000000"
          },
          "attributes": {
            "src": "/usr/local/bin/../share/yosys/xilinx/cells_sim.v:69.5-69.18"
          },
          "port_directions": {
            "DST": "input",
            "EN": "input",
            "SRC": "input"
          },
          "connections": {
            "DST": [ 2 ],
            "EN": [ "1" ],
            "SRC": [ 3 ]
          }
        }
      },
      "netnames": {
        "I": {
          "hide_name": 0,
          "bits": [ 3 ],
          "attributes": {
            "src": "/usr/local/bin/../share/yosys/xilinx/cells_sim.v:62.11-62.12"
          }
        },
        "O": {
          "hide_name": 0,
          "bits": [ 2 ],
          "attributes": {
            "iopad_external_pin": "00000000000000000000000000000001",
            "src": "/usr/local/bin/../share/yosys/xilinx/cells_sim.v:61.12-61.13"
          }
        }
      }
    },
```

It’s more or less just like our `top` module — input `I`, output `O`, with a few properties like `blackbox: 1`, saying it’s a basic unit, not a module with internal contents. `iopad_external_pin: 1` is also easy to understand. Parameters are specified with a default value, neat.

The confusing part might be the `specify` cell. This is actually a `specify`  block in the [cell simulation library](https://github.com/YosysHQ/yosys/blob/main/techlibs/xilinx/cells_sim.v) of Yosys:

```verilog
  specify
    (I => O) = 0;
  endspecify
```

Which means the time delay from the I and O is zero. And I think this information is either used in nextpnr-xilinx timing reports, or not used at all. The segment is then very easy to understand, or ignore, seeing the T_RISE and T_FALLs.

Can try a slightly more complex case, a sequential logic:

```verilog
`timescale 1ns / 1ps
 
module top(
    input clk,
    input i,
		output o
    );
    always @ (posedge clk) begin
        o <= i;
    end
endmodule
```

The result is:

```json
    "top": {
      "attributes": {
        "hdlname": "top",
        "top": "00000000000000000000000000000001",
        "src": "top.v:4.1-14.10"
      },
      "ports": {
        "clk": {
          "direction": "input",
          "bits": [ 2 ]
        },
        "i": {
          "direction": "input",
          "bits": [ 3 ]
        },
        "o": {
          "direction": "output",
          "bits": [ 4 ]
        }
      },
      "cells": {
        "$auto$clkbufmap.cc:261:execute$1708": {
          "hide_name": 1,
          "type": "BUFG",
          "parameters": {
          },
          "attributes": {
          },
          "port_directions": {
            "I": "input",
            "O": "output"
          },
          "connections": {
            "I": [ 5 ],
            "O": [ 6 ]
          }
        },
        "$auto$ff.cc:337:slice$1516": {
          "hide_name": 1,
          "type": "FDRE",
          "parameters": {
            "INIT": "x"
          },
          "attributes": {
            "module_not_derived": "00000000000000000000000000000001",
            "src": "top.v:11.5-13.8|/usr/local/bin/../share/yosys/xilinx/ff_map.v:68.41-68.95"
          },
          "port_directions": {
            "C": "input",
            "CE": "input",
            "D": "input",
            "Q": "output",
            "R": "input"
          },
          "connections": {
            "C": [ 6 ],
            "CE": [ "1" ],
            "D": [ 7 ],
            "Q": [ 8 ],
            "R": [ "0" ]
          }
        },
        "$iopadmap$top.clk": {
          "hide_name": 1,
          "type": "IBUF",
          "parameters": {
          },
          "attributes": {
            "keep": "00000000000000000000000000000001"
          },
          "port_directions": {
            "I": "input",
            "O": "output"
          },
          "connections": {
            "I": [ 2 ],
            "O": [ 5 ]
          }
        },
        "$iopadmap$top.i": {
          "hide_name": 1,
          "type": "IBUF",
          "parameters": {
          },
          "attributes": {
            "keep": "00000000000000000000000000000001"
          },
          "port_directions": {
            "I": "input",
            "O": "output"
          },
          "connections": {
            "I": [ 3 ],
            "O": [ 7 ]
          }
        },
        "$iopadmap$top.o": {
          "hide_name": 1,
          "type": "OBUF",
          "parameters": {
          },
          "attributes": {
            "keep": "00000000000000000000000000000001"
          },
          "port_directions": {
            "I": "input",
            "O": "output"
          },
          "connections": {
            "I": [ 8 ],
            "O": [ 4 ]
          }
        }
      },
      "netnames": {
        "$abc$1704$iopadmap$clk": {
          "hide_name": 1,
          "bits": [ 6 ],
          "attributes": {
          }
        },
        "$abc$1704$iopadmap$i": {
          "hide_name": 1,
          "bits": [ 7 ],
          "attributes": {
          }
        },
        "$abc$1704$iopadmap$o": {
          "hide_name": 1,
          "bits": [ 8 ],
          "attributes": {
          }
        },
        "$auto$clkbufmap.cc:262:execute$1709": {
          "hide_name": 1,
          "bits": [ 5 ],
          "attributes": {
          }
        },
        "clk": {
          "hide_name": 0,
          "bits": [ 2 ],
          "attributes": {
            "src": "top.v:5.11-5.14"
          }
        },
        "i": {
          "hide_name": 0,
          "bits": [ 3 ],
          "attributes": {
            "src": "top.v:6.11-6.12"
          }
        },
        "o": {
          "hide_name": 0,
          "bits": [ 4 ],
          "attributes": {
            "src": "top.v:7.9-7.10"
          }
        }
      }
    }
```

![](/MyBlog/images/sequential.png)

Flip-flops, or the `FDRE` primitives are used for Xilinx 7-series FPGAs to implement the `always @ (posedge clk)` sequential logics. And it’s not hard to see that numbers means wires, and string “0” and “1” mean logic levels: clock always enabled, and no reset.

Everything we write does not magically turn in to running hardware, but must be reflected to basic units we have on the FPGA vendor!

Understanding this is the first step down the ~~drain~~ rabbit hole.
