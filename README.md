``` Java
import java.util.LinkedList;

public class Graphs {
	private boolean dirigido;
	private int maxNodos;
	private int numVertices;
	private boolean matAd[][];
	
	public Graphs(int n, boolean dir) {
		this.maxNodos = 50;
		this.numVertices = n;
		this.dirigido = dir;
		this.matAd = new boolean[maxNodos][maxNodos];
	}
	
	public void insertArista(int i, int j){
		this.matAd[i][j] = true;
		if(dirigido == false)
			matAd[j][i] = true;
	}
	
	public void deleteArista(int i, int j){
		matAd[i][j] = false;
		if(dirigido == false)
			matAd[j][i] = false;
	}
	
	public void insertVertices(int n){
		if(maxNodos- numVertices < n)
			System.out.println("Error supera el maximo numero de nodos");
		else{
			for (int i = 0; i < numVertices; i++) {
				for (int j = numVertices; j < numVertices+n; j++) {
					matAd[i][j] = matAd[j][i] = false;
				}
			}
		}
		numVertices = numVertices+n;
	}
	
	public boolean existArista(int i, int j){
		if(matAd[i][j])
			return true;
		return false;
	}
	
	public void printGraph(){
		System.out.println("La matriz contiene: " + this.numVertices + "  vertices \n");
		for (int i = 0; i < numVertices; i++) {
			for (int j = 0; j < numVertices; j++) {
				if(matAd[i][j])
					System.out.print("  1");
				else
					System.out.print("  0");
			}
			System.out.println();
		}
	}
	//en un grafo dirigido
	//grado de entrada
	public int gradoInterno(int i){
		int cont=0;
		for (int j = 0; j < numVertices; j++) {
			if(matAd[j][i])
				cont++;
		}
		return cont;
	}
	//en un grafo dirigido
	//grado de salida
	public int gradoExterno(int i){
		int cont=0;
		for (int j = 0; j < numVertices; j++) {
			if(matAd[i][j])
				cont++;
		}
		return cont;
	}
	//en un grafo no dirigido
	public int grado(int i){
		return((gradoInterno(i)+gradoExterno(i))/2);
	}
	
	void profundidad(Graphs g){
		boolean[] visitados = new  boolean[g.numVertices];
		for (int i = 0; i < g.numVertices; i++) {
			visitados[i] = false;
		}
		for (int i = 0; i < g.numVertices; i++) {
			if(!visitados[i])
				recorrerProfundidad(g,i,visitados);
		}
	}

	private void recorrerProfundidad(Graphs g, int v, boolean[] visitados) {
		visitados[v] = true;
		System.out.print(v + "   ");
		for (int i = 0; i < g.numVertices; i++) {
			if((v != i) && g.existArista(v, i) && !visitados[i])
				recorrerProfundidad(g, i, visitados);
		}
	}
	void amplitud(Graphs g){
		LinkedList<Integer> cola = new LinkedList<Integer>();
		boolean[] visitados = new boolean[g.numVertices];
		int v;
		for (int i = 0; i < g.numVertices; i++) {
			visitados[i] = false;
		}
		
		for (int i = 0; i < g.numVertices; i++) {
			if(!visitados[i]){
				cola.offer(i);
				visitados[i] = true;
				while(!cola.isEmpty()){
					v = cola.poll();
					System.out.print (v + "   ");
					for (int j = 0; j < g.numVertices; j++) {
						if((v != j) && g.existArista(v, j) && !visitados[j]){
							cola.offer(j);
							visitados[j] = true;
						}
					}
				}
			}
		}
	}
}


```
