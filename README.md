# project-2.2
And16.hdl
CHIP Add16 {
    IN a[16], b[16];
    OUT out[16];

  PARTS:
  	And(a=a[0],b=b[0],out=out[0]);
  	And(a=a[1],b=b[1],out=out[1]);
  	And(a=a[2],b=b[2],out=out[2]);
  	And(a=a[3],b=b[3],out=out[3]);
  	And(a=a[4],b=b[4],out=out[4]);
  	And(a=a[5],b=b[5],out=out[5]);
  	And(a=a[6],b=b[6],out=out[6]);
  	And(a=a[7],b=b[7],out=out[7]);
  	And(a=a[8],b=b[8],out=out[8]);
  	And(a=a[9],b=b[9],out=out[9]);
  	And(a=a[10],b=b[10],out=out[10]);
  	And(a=a[11],b=b[11],out=out[11]);
  	And(a=a[12],b=b[12],out=out[12]);
  	And(a=a[13],b=b[13],out=out[13]);
  	And(a=a[14],b=b[14],out=out[14]);
  	And(a=a[15],b=b[15],out=out[15]);

    
}
![image](https://github.com/user-attachments/assets/0a82d491-593e-4b84-bcbc-3070139dd3a8)

3. ALU.hdl
CHIP ALU {

    IN  // 16-bit inputs:
        x[16], y[16],
        // Control bits:
        zx, // Zero the x input
        nx, // Negate the x input
        zy, // Zero the y input
        ny, // Negate the y input
        f,  // Function code: 1 for add, 0 for and
        no; // Negate the out output

    OUT // 16-bit output
        out[16],

        // ALU output flags
        zr, // 1 if out=0, 0 otherwise
        ng; // 1 if out<0, 0 otherwise

    BUILTIN ALU;
    Mux16(a=x,b[0..15]=false,sel=zx,out=zdx); //Zero the x
    Not16(in=zdx,out=notx);                  //Not the x
    Mux16(a=zdx,b=notx,sel=nx,out=ndx);      //chose x or notx
    // ditto for y
    Mux16(a=y,b[0..15]=false,sel=zy,out=zdy);
    Not16(in=zdy,out=noty);
    Mux16(a=zdy,b=noty,sel=ny,out=ndy);
    
    Add16(a=ndx,b=ndy,out=xplusy); //x+y
    And16(a=ndx,b=ndy,out=xandy);  //x&y
    Mux16(a=xandy,b=xplusy,sel=f,out=fxy);  //chose function
    
    Not16(in=fxy,out=nfxy);      //not the output or not
    Mux16(a=fxy,b=nfxy,sel=no,out=oo);  //chose which
    
    
    Or16Way(in=oo,out=o);  //for zr
    Not(in=o,out=zr);
    
    And16(a[0..15]=true,b=oo,out[15]=ng,out[0..14]=drop); //ng
    
    Or16(a=oo,b[0..15]=false,out=out); //oo=output
    }

![image](https://github.com/user-attachments/assets/bd8f57bd-6cb1-41af-8c8d-52128b1b0638)

