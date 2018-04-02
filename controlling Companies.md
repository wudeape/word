 ## 问题描述
控制公司

译 by TinyTony

有些公司是其他公司的部分拥有者，因为他们获得了其他公司发行的股票的一部分。例如，福特公司拥有马自达公司12%的股票。据说，如果至少满足了以下条件之一，公司A就可以控制公司B了：

公司A = 公司B。 
公司A拥有大于50%的公司B的股票。 
公司A控制K(K >= 1)个公司，记为C1, ..., CK，每个公司Ci拥有xi%的公司B的股票，并且x1+ .... + xK > 50%。 
你将被给予一系列的三对数（i，j，p），表明公司i享有公司j的p%的股票。计算所有的数对（h，s），表明公司h控制公司s。至多有100个公司。

写一个程序读入三对数（i，j，p），i，j和p是都在范围(1..100)的正整数，并且找出所有的数对（h，s），使得公司h控制公司s。

PROGRAM NAME: concom

INPUT FORMAT

第一行： N，表明接下来三对数的数量。 
第二行到第N+1行： 每行三个整数作为一个三对数（i，j，p），如上文所述。 

SAMPLE INPUT (file concom.in) 

3
1 2 80
2 3 80
3 1 20

OUTPUT FORMAT

输出零个或更多个的控制其他公司的公司。每行包括两个整数表明序号为第一个整数的公司控制了序号为第二个整数的公司。将输出的每行以第一个数字升序排列（并且第二个数字也升序排列来避免并列）。请不要输出控制自己的公司。

SAMPLE OUTPUT (file concom.out)

1 2
1 3
2 3


本体体现的就是一个枚举的过程，并且需要不断的进行更新数据

#include< stdio.h>
#include<stdlib.h>
#include <string.h>
#include<assert.h>

 # define NCOM 101

 int owns[NCOM][NCOM];

 /* [i,j]: how much of j do i and its controlled companies own?

 int controls[NCOM][NCOM];

 /* [i, j]: does i control j? */



/* update info: now i controls j */

  void addcontroler( int i, int j){
      int k;
       if(control[i][j])
       return ;
       controls[i][j] =1;

        for( k=0 ;k< NCOM ; k++)
         owns[i][k]+=owns[j][k];

          for (k =0 ;k< NOCOM ;k++)
            if( controls[k][i])
              addcontroler(k ,j);

               for ( k=0; k< NCOM ; k++)
                 if( own[i][k] >50)
                  addcontroler(i,k);

  }


* update info: i owns p% of j */
   void addowner( int i, int j, int p)
   {
      int k;
      for (k=0; k< NCOM ; k++)
      if( controls[k][i])
        owns[k][j]+= p;

         for ( k=0 ; k< NCOM ; k++)
           addcontroller( k,j);

   }
     void main(){

     FILE * fin, * fout;
      int i ,j ,n ,a, b ,p;
       fin = fopen ( "concom.in","r");
       fout = fopen( "concom.out","w");
       assert( fin! = NULL && fout! = NULL);

        for ( i=0 ; i<NCOM ;i++)
          controls[i][j]=1;
              fscanf ( fin ,"%d",&n);
              for ( int i=0; i<n;i++)
                fscanf ( fin ,"%d %d %d",&a, &b, &p);
                 addowner( a, b,p);

                  for ( i=0 ; i< NCOM ;i++)
                   for ( j=0  ; j< NCOM ;j++)
                    if( i! = j && controls[i][j])
                     fprintf( fout, "%d %d\n",i j);
                      return 0;

     }