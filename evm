module evm (
    input wire [3:0] voter_switch,
    input wire Push_Button,
    input wire voting_en,
    input wire reset,
    output reg [6:0] dout,
    output reg [3:0] output_LED,
    output reg invalid,
    output reg [4:0] v1,
    output reg [6:0] seg3,
    input wire sw1,
    input wire sw2,
    input wire sw3,
    input wire sw4,
    input wire swout
);

// Counters to count each party votes
reg [4:0] cnt_reg1 = 0;  // party1
reg [4:0] cnt_reg2 = 0;  // party2
reg [4:0] cnt_reg3 = 0;  // party3
reg [4:0] cnt_reg4 = 0;  // party4

// Counter for party1 votes
always @(posedge Push_Button or posedge reset) begin
    if (reset) begin
        cnt_reg1 <= 0;
    end else if (voter_switch == 4'b0001 && voting_en) begin
        cnt_reg1 <= cnt_reg1 + 1;
    end
end

// Counter for party2 votes
always @(posedge Push_Button or posedge reset) begin
    if (reset) begin
        cnt_reg2 <= 0;
    end else if (voter_switch == 4'b0010 && voting_en) begin
        cnt_reg2 <= cnt_reg2 + 1;
    end
end

// Counter for party3 votes
always @(posedge Push_Button or posedge reset) begin
    if (reset) begin
        cnt_reg3 <= 0;
    end else if (voter_switch == 4'b0100 && voting_en) begin
        cnt_reg3 <= cnt_reg3 + 1;
    end
end

// Counter for party4 votes
always @(posedge Push_Button or posedge reset) begin
    if (reset) begin
        cnt_reg4 <= 0;
    end else if (voter_switch == 4'b1000 && voting_en) begin
        cnt_reg4 <= cnt_reg4 + 1;
    end
end

// Final count (total number of votes)
always @(*) begin
    if (swout && voting_en) begin
        dout = cnt_reg1 + cnt_reg2 + cnt_reg3 + cnt_reg4;
    end else begin
        dout = 7'b0000000;
    end
end

// For displaying votes
always @(*) begin
    if (sw1 && !sw2 && !sw3 && !sw4 && !swout && voting_en) begin
        v1 = cnt_reg1;
        seg3 = 7'b1001111; // Display '1'
    end else if (!sw1 && sw2 && !sw3 && !sw4 && !swout && voting_en) begin
        v1 = cnt_reg2;
        seg3 = 7'b0010010; // Display '2'
    end else if (!sw1 && !sw2 && sw3 && !sw4 && !swout && voting_en) begin
        v1 = cnt_reg3;
        seg3 = 7'b0000110; // Display '3'
    end else if (!sw1 && !sw2 && !sw3 && sw4 && !swout && voting_en) begin
        v1 = cnt_reg4;
        seg3 = 7'b1001100; // Display '4'
    end else if (!sw1 && !sw2 && !sw3 && !sw4 && swout && voting_en) begin
        v1 = dout % 100;
        if (dout < 10) begin
            seg3 = 7'b1111001; // Display 'L' for low count
        end else begin
            seg3 = 7'b0000001; // Display 'H' for high count
        end
    end else begin
        v1 = 5'b00000;
        seg3 = 7'b0000001; // Default display
    end
end

endmodule
