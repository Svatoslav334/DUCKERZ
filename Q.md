graph TD
    Please use Mermaid graph markdown for diagram
    subgraph Control Logic
        CLK_MAIN[Main CLOCK] --> MasterCnt[2-bit Master Counter]
        MasterCnt -- 2 bits Select --> Decoder[Decoder 2-to-4]
        Decoder -- EN 0 --> Block0
        Decoder -- EN 1 --> Block1
        Decoder -- EN 2 --> Block2
        Decoder -- EN 3 --> Block3
    end

    subgraph Block 0 (Lines I0-I7)
        EN0[Enable 0] --> C0[3-bit Counter C0] & MUX0_EN[MUX0 Enable]
        CLK_MAIN --> C0
        C0 -- 3 bits addr --> MUX0[MUX 8-to-1]
        I0_7[Lines I0...I7] --> MUX0
    end

    subgraph Block 1 (Lines I8-I15)
        EN1[Enable 1] --> C1[3-bit Counter C1] & MUX1_EN[MUX1 Enable]
        CLK_MAIN --> C1
        C1 -- 3 bits addr --> MUX1[MUX 8-to-1]
        I8_15[Lines I8...I15] --> MUX1
    end
    
    subgraph Block 2 (Lines I16-I23)
        EN2[Enable 2] --> C2[3-bit Counter C2] & MUX2_EN[MUX2 Enable]
        CLK_MAIN --> C2
        C2 -- 3 bits addr --> MUX2[MUX 8-to-1]
        I16_23[Lines I16...I23] --> MUX2
    end

    subgraph Block 3 (Lines I24-I31)
        EN3[Enable 3] --> C3[3-bit Counter C3] & MUX3_EN[MUX3 Enable]
        CLK_MAIN --> C3
        C3 -- 3 bits addr --> MUX3[MUX 8-to-1]
        I24_31[Lines I24...I31] --> MUX3
    end

    subgraph Output Logic
        MUX0 -- bit --> OR_GATE[4-input OR Gate]
        MUX1 -- bit --> OR_GATE
        MUX2 -- bit --> OR_GATE
        MUX3 -- bit --> OR_GATE
        
        OR_GATE --> INT_FOUND[Interrupt Found Signal]

        %% Address generation (Optional, for completeness)
        MasterCnt -- High 2 bits --> ADDR_OUT[5-bit Interrupt Address]
        C0 -- Low 3 bits --> MUX_ADDR[MUX 4-to-1 (3 bit wide)]
        C1 --> MUX_ADDR
        C2 --> MUX_ADDR
        C3 --> MUX_ADDR
        MasterCnt --> MUX_ADDR
        MUX_ADDR --> ADDR_OUT
    end
