3.
	Primero hay que generar un linspace de posibles parámetros para probar. Si se usa un lambda pequeño es como no estar regularizando y puede haber overfitting. Si es grande se puede alejar demasiado de la función real y estar muy sesgado. Con cross validation se separa la base de entrenamiento en k folds y se entrena con k-1 folds. El que queda se usa para validar. Repetimos k veces sin repetir folds. Habiéndolo calculado tantas veces se elige el de menor ecm.	Es bueno no usar el conjunto de prueba que separamos al inicio para tener mas datos sobre los que probar que no haya overfitting 

4.

un k muy pequeño es más facil de computar porque iteras menos veces ya que son menos folds. El problema es que los resultados varían mucho entre diferentes realizaciones (alta varianza) ya que una parte demasiado grande de la muestra estaría siendo dejada afuera. Está claro que no es bueno usar demasiados pocos datos para entrenar.
Por otro lado, si k es muy grande es obviamente más costoso de computar pero además entre un fold y otro no cambia mucho la muestra de entrenamiento (si k=n cambiaria por un solo dato) lo cual va a ser problemático al computar los errores (están muy correlacionados). Pero en general aumentar k va a reducir la varianza que tenias con k chico, pero puede aumentar el sesgo. 
El caso extremo que mencionamos de k=n s ellama leave one out (LOOCV) y tiene esos problemas que mencionamos. Está entrenando el modelo n veces.

6.
LASSO descartó las variables ['PP09B' 'PP09C' 'V19_A' 'V19_B']. Las primeras son sobre si trabaja en otra ciudad que peude tener sentido. Pero las otras indican si un menor de 10 años sale a pedir o sale a trabajar para contribuir al hogar. Obviamente es una variable que deberia explicar muy bien la pobreza, no debería ser eleminada. En algun momento hicimos algo mal y nos dio este resultado.

