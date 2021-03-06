# Jacobi Iterations for the Laplace Equation with OpenGL interoperability

## Equazione di Laplace: Iterazioni di Jacobi

Consideriamo l'equazione di Laplace

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=\nabla^2T(x,y)=0." id="laplaceEquation">       [1]
</p>

in cui l'incognita <img src="https://render.githubusercontent.com/render/math?math=T(x,y)"> è una funzione di due variabili spaziali <img src="https://render.githubusercontent.com/render/math?math=(x,y)">. L'equazione di Laplace [\[1\]](#laplaceEquation) modella lo steady state di una funzione, definita in uno dominio two-dimensional, che rappresenta una particolare grandezza fisica. In casi pratici, <img src="https://render.githubusercontent.com/render/math?math=T(x,y)"> può rappresentare il calore, e l'equazione [\[1\]](#laplaceEquation) può essere usata per studiare come il calore si distribuisce in un certo dominio, fissate che siano le condizioni al contorno

Se espandiamo l'operatore Laplaciano <img src="https://render.githubusercontent.com/render/math?math=\nabla^2">, otteniamo

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=\frac{\partial^2 T(x,y)}{\partial x^2} %2B \frac{\partial^2 T(x,y)}{\partial y^2}=0." id="laplaceEquationExpanded">       [2]
</p>

Introduciamo a questo punto un grigliato di calcolo di <img src="https://render.githubusercontent.com/render/math?math=M\times N"> punti <img src="https://render.githubusercontent.com/render/math?math=(x_m, y_n)">, con <img src="https://render.githubusercontent.com/render/math?math=x_m=m \Delta x"> ed <img src="https://render.githubusercontent.com/render/math?math=y_n=n \Delta y">, con <img src="https://render.githubusercontent.com/render/math?math=m=0,\ldots,M-1"> ed <img src="https://render.githubusercontent.com/render/math?math=n=0,\ldots,N-1">. Infine, indichiamo con <img src="https://render.githubusercontent.com/render/math?math=T_{mn}"> i valori dell'incognita nei punti del grigliato introdotto, ossia <img src="https://render.githubusercontent.com/render/math?math=T(x_m,y_n)=T_{m,n}">.

Approssimando le derivate contenute nella [\[2\]](#laplaceEquationExpanded) con differenze finite e assumendo <img src="https://render.githubusercontent.com/render/math?math=\Delta x=\Delta y = 1">, si ha il seguente sistema di <img src="https://render.githubusercontent.com/render/math?math=M\times N"> equazioni lineari algebriche

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=T_{m %2B 1,n}-2T_{m,n} %2B T_{m-1,n} %2B T_{m,n %2B 1}-2T_{m,n} %2B T_{m,n-1}=0," id="laplaceEquationDiscretized">       [3]
</p>

con <img src="https://render.githubusercontent.com/render/math?math=m=0,\ldots,M-1"> ed <img src="https://render.githubusercontent.com/render/math?math=n=0,\ldots,N-1">.

Il sistema [\[3\]](#laplaceEquationDiscretized) può essere risolto utilizzando il metodo di Jacobi. Quest'ultimo è un metodo iterativo per risolvere un sistema di <img src="https://render.githubusercontent.com/render/math?math=P"> equazioni lineari in <img src="https://render.githubusercontent.com/render/math?math=P"> incognite <img src="https://render.githubusercontent.com/render/math?math=\mathbf{b}=\mathbf{A}\cdot \mathbf{x}">. Per illustrare il metodo di Jacobi, riscriviamo l'<img src="https://render.githubusercontent.com/render/math?math=i">-ma equazione del sistema lineare come

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=a_{i,1}x_1 %2B a_{i,2}x_2 %2B \ldots %2B a_{i,P}x_P=b_i," id="ithEquation">       [4]
</p>

where gli <img src="https://render.githubusercontent.com/render/math?math=a_{i,p}">'s sono gli elementi della matrice <img src="https://render.githubusercontent.com/render/math?math=\mathbf{A}"> e gli <img src="https://render.githubusercontent.com/render/math?math=b_i">'s sono gli elementi del vettore <img src="https://render.githubusercontent.com/render/math?math=\mathbf{b}">. Il valore di <img src="https://render.githubusercontent.com/render/math?math=x_i"> può dunque essere espresso come

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=x_i = \frac{1}{a_{i,i}}\left[b_i-\sum_{j\neq i}a_{i,j}x_j\right]." id="ithUnknown">       [5]
</p>

Il metodo Jacobi per risolvere un sistema di equazioni lineari definisce iterazioni per le quali, data la soluzione al passo <img src="https://render.githubusercontent.com/render/math?math=k">-mo, diciamo <img src="https://render.githubusercontent.com/render/math?math=\mathbf{x}^k">, la soluzione al passo successivo viene espressa come

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=x_i^k = \frac{1}{a_{i,i}}\left[b_i-\sum_{j\neq i}a_{i,j}x_j^{k-1}\right]." id="ithUnknownJacobi">       [6]
</p>

Ispirati dalla [\[6\]](#ithUnknownJacobi), il metodo di Jacobi per la soluzione numerica dell'equazione di Laplace consiste in:

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=T_{m,n}^k = 0.25\left[T_{m-1,n}^{k-1} %2B T_{m %2B 1,n}^{k-1} %2B T_{m,n-1}^{k-1} %2B T_{m,n %2B 1}^{k-1}\right]." id="LaplaceJacobi">       [7]
</p>

La regola di update [\[7\]](#LaplaceJacobi) consiste dunque nel calcolo dello stencil nella figura seguente

<p align="center">
  <img src="stencilLaplace.png" width="400" id="stencilLaplace">
  <br>
     <em>Figure 1. Computational stencil for Laplace equation.</em>
</p>

where the south <img src="https://render.githubusercontent.com/render/math?math=T_s">, north <img src="https://render.githubusercontent.com/render/math?math=T_n">, west <img src="https://render.githubusercontent.com/render/math?math=T_w"> and east <img src="https://render.githubusercontent.com/render/math?math=T_e"> elements correspond to <img src="https://render.githubusercontent.com/render/math?math=T_{m-1,n}">, <img src="https://render.githubusercontent.com/render/math?math=T_{m %2B 1,n}">, <img src="https://render.githubusercontent.com/render/math?math=T_{m,n-1}"> and <img src="https://render.githubusercontent.com/render/math?math=T_{m,n %2B 1}">.

Per una risoluzione univoca dell'equazione di Laplace, abbiamo bisogno di condizioni al contorno. In questo esempio, immagineremo di avere un pipe centrato in un punto di coordinate <img src="https://render.githubusercontent.com/render/math?math=(x,y)"> e di raggio pari a <img src="https://render.githubusercontent.com/render/math?math=r"> che scorre ortogonalmente al solution domain. Il pipe si troverà ad una temperatura pari a <img src="https://render.githubusercontent.com/render/math?math=T_{pipe}">. Inoltre, i bordi sinistro, destro e superiore sono assunti avere una temperatura pari a <img src="https://render.githubusercontent.com/render/math?math=T_{air}">, mentre il bordo inferiore è assunto avere una temperatura pari a <img src="https://render.githubusercontent.com/render/math?math=T_{ground}">. In questo modo, modellizziamo un pipe che scorre in aria e posizionato vicino al suolo.

Come initial guess del metodo iterativo, viene scelta una temperatura che soddisfa le condizioni al contorno e nel resto del dominio vale `T_{air}`.

### Iterazioni di Jacobi per l'equazione di Laplace: implementazione CPU

Come step preparatorio ad illustrare lo schema implementativo su GPU, illustriamone dapprima la soluzione CPU contenuta nel file `heatEquationCPU.rar`.

Il `main` program è il seguente:

``` c++
int main() {

	float* h_temperature = (float*)malloc(W * H * sizeof(float));
	float* h_temperature_new = (float*)malloc(W * H * sizeof(float));

	resetTemperatureCPU(h_temperature, W, H, bc);

	for (int iter = 0; iter < MAX_NUM_ITERS / 2; iter++) {
		temperatureUpdateCPU(h_temperature, h_temperature_new, W, H, bc);
		temperatureUpdateCPU(h_temperature_new, h_temperature, W, H, bc);
	}

	saveCPUrealtxt(h_temperature, ".\\CPU_result.txt", W * H);

	return 0;
}
```

Come si può vedere, vengono definite due matrici, cioè `h_temperature` ed `h_temperature_new`, di dimensioni `W` e `H`, che dovranno contenere la distribuzione di temperatura alla generica iterazione `k` e `k %2B 1`, rispettivamente. La funzione `resetTemperatureCPU` fissa il valore dell'inizial guess, mentre `temperatureUpdateCPU` effettua l'update della temperatura, ossia valuta la regola di aggiornamento [\[7\]](#LaplaceJacobi). Si noti come il `for` loop che implementa le iterazioni effettui un numero di cicli pari a `MAX_NUM_ITERS / 2`, mentre ogni ciclo effettui due iterazioni. In particolare, la funzione `temperatureUpdateCPU` viene chiamata con parametri di ingresso `h_temperature` ed `h_temperature_new` scambiati passando da un'invocazione all'altra. E' da notare che, a valle della prima invocazione, `h_temperature_new` diventa l'array più aggiornato che, nell'iterazione successiva, dovrebbe diventare quello al passo precedente per poter essere aggiornato successivamente a sua volta. Per ottenere ciò, a valle della prima invocazione di `temperatureUpdateCPU`, gli array `h_temperature` ed `h_temperature_new` andrebbero swappati. Invocando `temperatureUpdateCPU` con parametri scambiati questo risultato si ottiene implicitamente. 

Infine, il risultato dell'elaborazione è salvato in un file testo invocando la funzione `saveCPUrealtxt`. Tale funzione fa parte del repository [CUDA-Utilities](https://vitalitylearning2021.github.io/CUDA-Utilities/).

La funzione `resetTemperatureCPU` effettua il reset della temperatura ad un valore di <img src="https://render.githubusercontent.com/render/math?math=20^\circ">:

``` c++
void resetTemperatureCPU(float* h_temperature, int width, int height, ProblemParameters bc) {

	for (int j = 0; j < height; j++)
		for (int i = 0; i < width; i++) {
			const int idx = j * width + i;
			h_temperature[idx] = bc.T_air;
		}
}
```

Infine, la funzione `temperatureUpdateCPU` è la seguente:

``` c++
void temperatureUpdateCPU(float* __restrict h_T, float* __restrict h_T_new, const int width, const int height, const ProblemParameters bc) {

	// --- Only update "interior" (not boundary) node points
	for (int j = 0; j < height; j++)
		for (int i = 0; i < width; i++) {
			const int idx = j * width + i;
			if ((i > 0) && (i < width - 1) && (j > 0) && (j < height - 1))
				h_T_new[idx] = 0.25f * (h_T[idx - 1] +
					h_T[idx + 1] +
					h_T[idx + width] +
					h_T[idx - width]);
			setBoundaryConditions(h_T_new, width, height, idx, i, j, bc);
		}
}
```

Essa meramente implementa la regola di update [\[7\]](#LaplaceJacobi). E' da notare come i valori di temperatura acceduti distino `1` per l'indice `n` e `width` per l'indice `m`. Ciò è dovuto al fatto che la matrice delle temperature è semanticamente bidimensionale, mentre essa è memorizzata in uno spazio di memoria lineare.

Infine, la funzione `setBoundaryConditions` fissa le condizioni al contorno sui bordi del dominio e sul pipe:

``` c++
void setBoundaryConditions(float* d_T, const int width, const int height, const int idx,
	const int tidx, const int tidy, const ProblemParameters bc) {

	// --- Set the pipe temperature to T_pipe and return
	float distanceFromPipeCenterSquared = ((tidx - bc.x) * (tidx - bc.x) + (tidy - bc.y) * (tidy - bc.y));
	if (distanceFromPipeCenterSquared < bc.radius * bc.radius) d_T[idx] = bc.T_pipe;

	// --- Set the left, right and upper border temperature to T_air and return
	if ((tidx == 0) || (tidx == width - 1) || (tidy == 0)) d_T[idx] = bc.T_air;

	// --- Set the lower border temperature to T_ground and return
	if (tidy == height - 1) d_T[idx] = bc.T_ground;

}
```



Assumendo <img src="https://render.githubusercontent.com/render/math?math=\Delta t=1"> e <img src="https://render.githubusercontent.com/render/math?math=\Delta x=\Delta y=1">,  l'equazione [\[3\]](#heatEquationDiscretized) definisce la seguente formula di aggiornamento:

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=\frac{T_{m,n}^{k %2B 1}-T_{m,n}^{k}}{\Delta t}=\frac{T_{m %2B 1,n}^{k}-2T_{m,n}^{k} %2B T_{m-1,n}^{k}}{\Delta x^2} %2B \frac{T_{m,n %2B 1}^{k}-2T_{m,n}^{k} %2B T_{m,n-1}^{k}}{\Delta y^2}." id="heatEquationUpdate">       [4]
</p>

## Equazione del calore

Consideriamo l'equazione differenziale alle derivate parziali

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=\frac{\partial T(x,y,t)}{\partial t}=\nabla^2T(x,y,t)." id="heatEquation">       [1]
</p>

in cui l'incognita <img src="https://render.githubusercontent.com/render/math?math=T(x,y,t)"> è una funzione di due variabili spaziali <img src="https://render.githubusercontent.com/render/math?math=(x,y)"> e di una variabile temporale <img src="https://render.githubusercontent.com/render/math?math=t">. Per una risoluzione univoca, abbiamo bisogno di una condizione iniziale e di condizioni al contorno. Ricordando di essere maggiormente interessati ad illustrare la tecnica programmativa di risoluzione di PDEs, non ci preoccuperemo di questo problema e fisseremo queste condizioni in seguito. Notiamo solo qui che, essendo capaci di risolvere l'equazione [\[1\]](#heatEquation), saremmo capaci di modellare la diffusione del calore in un determinato dominio bidimensionale.

https://www.dais.unive.it/~calpar/New_HPC_course/AA16-17/Project-Jacobi.pdf

https://skill-lync.com/student-projects/Steady-and-Transient-analysis-of-2D-heat-equation-with-implicit-and-explicit-methods-using-Jacobi-Gauss-Seidel-and-SOR-iterative-solvers-in-Matlab-15061

https://www.dsi.unive.it/~calpar/6_Progetto-02-03.pdf

https://github.com/ugovaretto-accel/cuda-opengl-interop/blob/master/texture3d-write/simpleGL3DSurfaceWrite.cpp

https://gist.github.com/allanmac/4ff11985c3562830989f

https://www.3dgep.com/opengl-interoperability-with-cuda/

https://www.3dgep.com/introduction-opengl/


Eliminare freeglutd.lib: https://stackoverflow.com/questions/29110985/why-is-visual-studio-trying-to-link-freeglutd-lib

https://github.com/prabindh/nengl/tree/master/cuda/ogl4

https://www.informit.com/articles/article.aspx?p=2455391&seqNum=2

https://www.adoclib.com/blog/cuda-opengl-interop-writing-to-surface-object-does-not-erase-previous-contents.html

http://www.mat.unimi.it/users/sansotte/cuda/CUDA_by_Example.pdf

https://sourceforge.net/projects/glew/

http://freeglut.sourceforge.net/ (da qui ci sono i link al download; devi andare in prepacked releases, vedi https://www.transmissionzero.co.uk/software/freeglut-devel/)

Linkare glew32s.lib e freeglut.lib

Spostare freeglut.dd nella directory dei file sorgenti di Visual Studio. C'è un modo per fare il link automatico indicando la directory della dll?

Vedi anche SimpleGL tra gli esempi CUDA.
