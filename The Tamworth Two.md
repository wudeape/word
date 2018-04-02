##  The Tamworth two
两只塔姆沃斯牛

译 by Jure
BOI '98? - Richard Forster 

两只牛在森林里故意走丢了。农民John开始用他的专家技术追捕这两头牛。你的任务是模拟他们的行为(牛和John)。 

追击在10x10的平面网格内进行。一个格子可以是： 

一个障碍物, 
两头牛(它们总在一起), 或者 
农民John. 
两头牛和农民John可以在同一个格子内(当他们相遇时)，但是他们都不能进入有障碍的格子。 

一个格子可以是： 

. 空地 
* 障碍物 
C 两头牛 
F 农民John 
这里有一个地图的例子:: 

*...*.....
......*...
...*...*..
..........
...*.F....
*.....*...
...*......
..C......*
...*.*....
.*.*......


牛在地图里以固定的方式游荡。每分钟，它们可以向前移动或是转弯。如果前方无障碍且不会离开地图，它们会按照原来的方向前进一步。否则它们会用这一分钟顺时针转90度。 

农民John, 深知牛的移动方法，他也这么移动。 

每次(每分钟)农民John和两头牛的移动是同时的。如果他们在移动的时候穿过对方，但是没有在同一格相遇，我们不认为他们相遇了。当他们在某分钟末在某格子相遇，那么追捕结束。开始时，John和牛都面向北方。

PROGRAM NAME: ttwo
INPUT FORMAT
Lines 1-10: 
每行10个字符，表示如上文描述的地图。


SAMPLE INPUT (file ttwo.in) 
*...*.....
......*...
...*...*..
..........
...*.F....
*.....*...
...*......
..C......*
...*.*....
.*.*......
OUTPUT FORMAT
输出一个数字，表示John需要多少时间才能抓住牛们。输出0，如果John无法抓住牛。

SAMPLE OUTPUT (file ttwo.out)
49

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <assert.h>

char grid[10][10];

/* delta x, delta y position for moving north, east, south, west */
int deltax[] = { 0, 1, 0, -1 };
int deltay[] = { -1, 0, 1, 0 };

void
move(int *x, int *y, int *dir)
{
 int nx, ny;

 nx = *x+deltax[*dir];
 ny = *y+deltay[*dir];

 if(nx < 0 || nx >= 10 || ny < 0 || ny >= 10 || grid[ny][nx] == '*')
  *dir = (*dir + 1) % 4;
 else {
  *x = nx;
  *y = ny;
 }
}

void
main(void)
{
 FILE *fin, *fout;
 char buf[100];
 int i, x, y;
 int cowx, cowy, johnx, johny, cowdir, johndir;

 fin = fopen("ttwo.in", "r");
 fout = fopen("ttwo.out", "w");
 assert(fin != NULL && fout != NULL);

 cowx = cowy = johnx = johny = -1;

 for(y=0; y<10; y++) {
  fgets(buf, sizeof buf, fin);
  for(x=0; x<10; x++) {
   grid[y][x] = buf[x];
   if(buf[x] == 'C') {
    cowx = x;
    cowy = y;
    grid[y][x] = '.';
   }
   if(buf[x] == 'F') {
    johnx = x;
    johny = y;
    grid[y][x] = '.';
   }
  }
 }

 assert(cowx >= 0 && cowy >= 0 && johnx >= 0 && johny >= 0);

 cowdir = johndir = 0; /* north */

 for(i=0; i<160000 && (cowx != johnx || cowy != johny); i++) {
  move(&cowx, &cowy, &cowdir);
  move(&johnx, &johny, &johndir);
 }

 if(cowx == johnx && cowy == johny)
  fprintf(fout, "%d\n", i);
 else
  fprintf(fout, "0\n");
 exit(0);
}