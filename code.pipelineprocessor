module pipelined_processor(
    input wire clk,
    input wire rst
);

    // Pipeline stages: Fetch, Decode, Execute, Writeback

    // Instruction signals
    reg [15:0] instruction;
    reg [15:0] reg_file [0:7];

    // Pipeline registers
    reg [15:0] IF_ID_instr;
    reg [15:0] ID_EX_instr;
    reg [15:0] EX_WB_result;
    reg [2:0]  EX_WB_dest;

    // Fetch stage
    always @(posedge clk or posedge rst) begin
        if (rst) begin
            IF_ID_instr <= 16'b0;
        end else begin
            IF_ID_instr <= instruction; // Assume instruction memory here
        end
    end

    // Decode stage
    wire [2:0] rs = IF_ID_instr[11:9];
    wire [2:0] rt = IF_ID_instr[8:6];
    wire [2:0] rd = IF_ID_instr[5:3];
    wire [2:0] opcode = IF_ID_instr[15:13];

    always @(posedge clk or posedge rst) begin
        if (rst) begin
            ID_EX_instr <= 16'b0;
        end else begin
            ID_EX_instr <= IF_ID_instr;
        end
    end

    // Execute stage
    reg [15:0] alu_result;

    always @(*) begin
        case (opcode)
            3'b000: alu_result = reg_file[rs] + reg_file[rt]; // ADD
            3'b001: alu_result = reg_file[rs] - reg_file[rt]; // SUB
            3'b010: alu_result = reg_file[rt];               // LOAD
            default: alu_result = 16'b0;
        endcase
    end

    always @(posedge clk or posedge rst) begin
        if (rst) begin
            EX_WB_result <= 16'b0;
            EX_WB_dest <= 3'b0;
        end else begin
            EX_WB_result <= alu_result;
            EX_WB_dest <= rd;
        end
    end

    // Writeback stage
    always @(posedge clk) begin
        if (EX_WB_dest != 3'b0) begin
            reg_file[EX_WB_dest] <= EX_WB_result;
        end
    end
endmodule
