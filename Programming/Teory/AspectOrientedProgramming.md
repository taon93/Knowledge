## Aspekt Oriented Programming: 
Jest to paradygmat programmowania w którym wydzielamy niektóre elementy programu (tak zwane cross-cutting concerns)
do pojedynczych modułów - każdy modół jest odpowiedzialny za tylko jeden aspekt - aspekty nie mają znaczenia z punktu
widzenia biznesowego i są niezależne od warstwy programu - to takie rzeczy jak logowanie, tranzakcyjność, security, etc.
Aspekty nie są dodawane bezpośrednio przez programistę, a raczej są dodawane do wykonania kodu za pomocą hooków: 
definiujemy join point (w springu niemal zawsze jest to poziom metody) który jest anchorem dla hooków - za każdym razem 
jak dany join point jest wywoływany to automatycznie wywołują się też metody aspektów, skorelowane z danym hookiem. 
Join pointy są związane z pointcutami - definiują one dla jakich join pointów ma być wywołana dana metoda - można to rozumieć
jako określenie że zestaw metod ma wspólny cross-cutting concer jednego typu, pointcut agreguje ten cross cutting concern. 
**Advice** - to właśnie hook - definiuje że coś ma być wywołane przed/po lub wokół danej metody, jak rozumiem w springu 
jest to zrobione w taki sposób że jeżeli metoda ma jakąś annotacje to staje się ona join pointem, egzekucją wszystkich 
adnotacji jednego typu kieruje pojedynczy pointcut, natomiast sama implementacja frameworka powoduje że metoda jest owrapowana 
w zewnętrzną metodę która dodaje dodatkową implementację z dodatkowym działaniem, w zależności od andotacji, to działanie może 
być wywołane bezpośrednio przy wejściu do funkcji, po wyjściu z niej, lub w obydwu tych przypadkach. Ta aplikacja proxy do metod/klass
by zaimplementować działanie jakiegoś aspektu nosi nazwę **weaving**

