//including necessary libraries
#include <stdbool.h>
#include <stdlib.h>

const short int BLACK = 0x0000;
const short int YELLOW = 0xFF20;
const short int BLUE = 0x20FF;

//global variable, used in multiple functiosn
int pixel_buffer_start; 

//declaring draw line and clear screen
void draw_line(int x0, int y0, int x1, int y1, short int color);
void swapnumber(int* x, int* y);
void plot_pixel(int x, int y, short int line_color);
void draw_cap(int x0, int x1, int y);
void draw_res(int x0, int x1, int y);
void draw_ground(int x0, int y1, int y);
void draw_circle(int xc, int yc, int r, short int line_colour);
void draw_half_circle(int xc, int yc, int r, short int line_colour);
void draw_power(int x0, int y0, int r);
void draw_current(int x0, int y0, int r);
void draw_inductor(int x0, int x1, int y);

void draw_one(int x0, int y0, int y1);
void draw_two(int xc, int yc, int r);
void clear_screen();
 
int main(void)
{
volatile int * pixel_ctrl_ptr = (int *)0xFF203020;
/* Read location of the pixel buffer from the pixel buffer controller */
pixel_buffer_start = *pixel_ctrl_ptr;
	
clear_screen();
draw_cap(10, 60, 50);
draw_res(100, 150, 150);
draw_ground(200, 130, 200);
draw_ground(50, 30, 30);
draw_power(100, 150, 15);
draw_current(200, 200, 15);
draw_half_circle(100, 100, 20, BLUE);
draw_inductor(50, 150, 160);
draw_one(100, 50, 80);
draw_two(150, 100, 20);
	


}


//plots the colour of a pixel at point (x, y)
void plot_pixel(int x, int y, short int line_color)
{
volatile short int *one_pixel_address;
one_pixel_address = pixel_buffer_start + (y << 10) + (x << 1);

*one_pixel_address = line_color;
}

//draws line from (x0, y0) to (x1, y1) with colour color
void draw_line(int x0, int y0, int x1, int y1, short int color){
	int y_step;
	
	bool is_steep=abs(y1-y0)>abs(x1-x0);
	if(is_steep){
		swapnumber(&x0, &y0);
		swapnumber(&x1, &y1);
	}
	if(x0>x1){
		swapnumber(&x0, &x1); 
		swapnumber(&y0, &y1);
	}
	int deltax=x1-x0;
	int deltay=abs(y1-y0); 
	int error=-(deltax/2); 
	int y=y0;
	if(y0<y1){
		y_step=1;
	}
	else{
		y_step=-1;
	}
	
	for(int x=x0;x<=x1;x++){
		if(is_steep){
			plot_pixel(y,x, color);
		}
		else{
			plot_pixel(x,y,color);
		}
		error=error+deltay;
		if(error>=0){
			y=y+y_step;
			error=error-deltax;
		}
	}
}


//swaps two numbers for Bresenham’s algorithm
void swapnumber(int* x, int* y){
	int temp= *x; 
	*x=*y; 
	*y=temp;
}
	
void clear_screen(){
	for (int x = 0; x< 320; x++){
		for (int y = 0; y < 240; y++){
			plot_pixel(x, y, BLACK);
			
		}
	}
	
}

void draw_cap(int x0, int x1, int y){
	int cap_size = (1.0/3.0) * (x1 - x0);
	draw_line(x0, y, x0 + cap_size, y, YELLOW);
	draw_line(x0 + 2 * cap_size, y, x1, y, YELLOW);
	draw_line(x0 + cap_size, y - cap_size, x0 + cap_size, y + cap_size, YELLOW);
	draw_line(x0 + 2 * cap_size, y - cap_size, x0 + 2 * cap_size, y + cap_size, YELLOW);
}	

void draw_res(int x0, int x1, int y){
	int res_size = abs((x1 - x0)/3);
	int last_pos_x;
	draw_line (x0  - res_size/2, y, x0 - res_size, y, YELLOW);
	draw_line(x0 - res_size/2, y, x0, y - res_size, YELLOW);
	for (int i = 0; i < 6; i = i + 2){
		draw_line(x0 + (i + 2) * res_size, y - res_size, x0 + (i + 1) * res_size, y + res_size, YELLOW);
		draw_line(x0 + i * res_size, y - res_size, x0 + (i + 1) * res_size, y + res_size, YELLOW);
		last_pos_x = x0 + (i + 2) * res_size;
	}
	draw_line(last_pos_x, y - res_size, last_pos_x + res_size/2, y, YELLOW);
	draw_line(last_pos_x + res_size/2, y, last_pos_x + res_size, y, YELLOW);

}

void draw_ground(int x0, int x1, int y){
	int ground_size = (x1 - x0)/6;
	draw_line(x0, y, x0 + 6 * ground_size, y, YELLOW);
	draw_line(x0 + ground_size, y - ground_size, x0 + 5 * ground_size, y - ground_size, YELLOW);
	draw_line(x0 + 2 * ground_size, y - 2 * ground_size, x0 + 4 * ground_size, y - 2 * ground_size, YELLOW);
	
}

void draw_power(int x0, int y0, int r){
	draw_circle(x0, y0, r, BLUE);
	draw_line(x0 - r/2, y0 - r/2, x0, y0 + r/2, BLUE);
	draw_line(x0, y0 + r/2, x0 + r/2, y0 - r/2, BLUE);
		//top line
	draw_line(x0, y0 - 2 * r, x0, y0 - r, BLUE);
	
	//bottom line
	draw_line(x0, y0 + 2 * r, x0, y0 + r, BLUE);
	
	
	
}

void draw_current (int x0, int y0, int r){
	draw_circle(x0, y0, r, BLUE);
	draw_line(x0, y0+r/2, x0, y0-r/2, BLUE);
	draw_line(x0 - r/2, y0, x0, y0 - r/2, BLUE);
	draw_line(x0, y0 - r/2, x0 + r/2, y0, BLUE);
	//top line
	draw_line(x0, y0 - 2 * r, x0, y0 - r, BLUE);
	
	//bottom line
	draw_line(x0, y0 + 2 * r, x0, y0 + r, BLUE);
	
}

void draw_inductor(int x0, int x1, int y){
	draw_line(x0 - 50, y - 20, x0 - 20, y - 20, BLUE);
	for (int i = 0; i < 4; i++){
		draw_half_circle(x0 + i * 40, y, 30, BLUE);
	}
	draw_line(x0 + 140, y - 20, x0 + 170, y - 20, BLUE);
		
}
void draw_circle(int xc, int yc, int r, short int line_colour)
{
    int x = 0;
    int y = r;
    int p = 1 - r;   // Decision parameter

    while (x <= y)
    {
        // Plot the 8 symmetric points

        plot_pixel(xc + x, yc + y, line_colour);
        plot_pixel(xc - x, yc + y, line_colour);
        plot_pixel(xc + x, yc - y, line_colour);
        plot_pixel(xc - x, yc - y, line_colour);

        plot_pixel(xc + y, yc + x, line_colour);
        plot_pixel(xc - y, yc + x, line_colour);
        plot_pixel(xc + y, yc - x, line_colour);
        plot_pixel(xc - y, yc - x, line_colour);

        x++;

        if (p < 0)
        {
            // Midpoint is inside circle
            p = p + 2 * x + 1;
        }
        else
        {
            // Midpoint is outside/on circle
            y--;
            p = p + 2 * x + 1 - 2 * y;
        }
    }
}

void draw_one(int x0, int y0, int y1){
	int one_length = (y1 - y0)/3;
	draw_line(x0, y0, x0, y1, YELLOW);
	draw_line(x0 - one_length, y0 + one_length, x0, y0, YELLOW);
	draw_line(x0 - one_length, y1, x0 + one_length, y1, YELLOW);
	
	
}

void draw_two(int xc, int yc, int r){
	draw_half_circle(xc, yc, r, YELLOW);
	draw_line(xc + r - 5, yc - 13, xc - r, yc +r, YELLOW);
	draw_line(xc - r, yc + r, xc + r, yc +r, YELLOW);
	
	
}

void draw_three(
void draw_half_circle(int xc, int yc, int r, short int line_colour){
    int x = 0;
    int y = r;
    int p = 1 - r;   // Decision parameter

    while (x <= y)
    {
        // Plot the 8 symmetric points


        plot_pixel(xc + x, yc - y, line_colour);
        plot_pixel(xc - x, yc - y, line_colour);

        x++;

        if (p < 0)
        {
            // Midpoint is inside circle
            p = p + 2 * x + 1;
        }
        else
        {
            // Midpoint is outside/on circle
            y--;
            p = p + 2 * x + 1 - 2 * y;
        }
    }	
	
}




	

	
