===========1024 / 10===========
v1:
1 -> 8.246138265
2 -> 4.4602931120000004
4 -> 2.571420436
8 -> 2.366719996

v2:
1 -> 5.144668061
2 -> 4.120354587
4 -> 2.794084267
8 -> 2.487163207
===============================

LIMITES:

En programmant la première version j'ai d'abord essayé de partager le monde généré en plusieurs sous mondes, et faire sous-traiter à chacun, toutes ses forces nécessaires. Cette approche naïve me causa dès le départ des problèmes de performances : en effet je me suis très vite rendu compte qu'il fallait partager non seulement la data généré, mais aussi les forces, sur lesquels je devais effectuer un allgather(). Quant à la deuxième version, je me suis retrouvé obligé à broadcaster l'entièreté du tableau de force à chacun des processus, car pour calculer fij = -fij, il fallait que chacun des processus ait accès à l'entièreté du tableau : la version parallélisée du code va donc faire faire calculer par exemple la planète 1024 avec 1023, 1022 etc... La répartition des données à calculer engendre le fait que plus on a de processeur, plus les calculs se rallongent (car chacun des processus doit faire le calcul sur l'entièreté du tableau). En effet, selon la loi d'Amdahl, l'exécution d'un programme parallèle dépend directement du code séquentiel parallélisé.
