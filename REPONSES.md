# TP3 — Réponses

**Binôme :** Fatma Achouri
> Répondez sous chaque question. Les mesures (durées, pourcentages) sont celles de **votre** machine.

## Partie 1 — Pyramide des tests

**Q1.** Pour chacun des dossiers `tests/unit`, `tests/component`, `tests/integration` et `tests/e2e`, indiquez : le niveau de la pyramide, l'outil utilisé, et si la stack Docker doit tourner pour l'exécuter.


Les tests du dossier tests/unit correspondent aux tests unitaires. Ils utilisent l'outil Vitest et ne nécessitent pas le démarrage de la stack Docker.

Les tests du dossier tests/component correspondent aux tests de composants. Ils utilisent Vitest, React Testing Library et jsdom pour tester les composants React. Ils ne nécessitent pas non plus la stack Docker.

Les tests du dossier tests/integration correspondent aux tests d'intégration. Ils utilisent Vitest pour vérifier les échanges entre les différentes parties de l'application à travers des appels API HTTP. Ils nécessitent le démarrage de la stack Docker, notamment Strapi.

Enfin, les tests du dossier tests/e2e correspondent aux tests de bout en bout. Ils utilisent Playwright pour simuler les actions d'un utilisateur dans le navigateur. Ils nécessitent le démarrage du frontend et des services Docker nécessaires au fonctionnement de l'application.












**Q2.** Pourquoi la pyramide des tests est-elle large à la base et étroite au sommet ? Donnez au moins deux critères.


La pyramide est large à la base et étroite au sommet parce que :
- Les tests unitaires sont rapides et peu coûteux : on peut en exécuter beaucoup.
- Les tests d'intégration sont plus lents et nécessitent plusieurs services.
- Les tests E2E sont les plus lents et les plus coûteux, car ils simulent un véritable utilisateur.
- Les tests de haut niveau sont généralement plus fragiles et plus difficiles à maintenir.











## Partie 2 — Tests unitaires

**Q3.** Combien de tests sont exécutés et combien échouent ? Recopiez le message d'échec et expliquez ce que signifient `Expected` et `Received`.


Le nombre de tests et d'échecs doit être relevé dans l'exécution initiale, avant la correction du bug.

Après correction, les résultats obtenus dans notre TP étaient de 21 tests réussis et 0 échec.

Dans un message d'échec :
- Expected : valeur attendue par le test.
- Received : valeur réellement retournée par le programme.
La différence entre les deux indique que le résultat du programme ne correspond pas au résultat attendu.


**Q4.** Quelle était la cause du bug dans `formatDuration` ? Expliquez pourquoi la correction fournie le résout.

Le bug se trouvait dans la logique de la fonction formatDuration. La fonction ne traitait pas correctement un ou plusieurs cas limites de durée. La correction adapte le calcul et le formatage pour retourner le résultat attendu, notamment dans les cas particuliers testés.






**Q5.** Avant d'ajouter des tests : relevez la couverture (lignes, branches, fonctions) de `lib/cours.ts`. D'après le rapport HTML, quelles lignes ou branches ne sont pas couvertes ?

Avant l'ajout des tests, il faut relever dans le rapport HTML de couverture de Vitest :
- La couverture des lignes.
- La couverture des branches.
- La couverture des fonctions.
Les lignes non exécutées apparaissent en rouge et les lignes partiellement couvertes en jaune. Les branches non couvertes correspondent aux chemins conditionnels qui n'ont pas été testés.






**Q6.** Après l'ajout des deux tests fournis : quelle est la nouvelle couverture de `lib/cours.ts` ? Quelles lignes ou branches ces tests exécutent-ils qui ne l'étaient pas ? Reste-t-il quelque chose de non couvert, et pourquoi ?


Après l'ajout des deux tests, la couverture augmente parce que ces tests exécutent des cas qui n'étaient pas vérifiés auparavant.

Ils permettent notamment de tester les chemins supplémentaires de formatDuration. Il peut rester des lignes ou des branches non couvertes si certains cas particuliers ne sont pas encore testés.



**Q7.** Quelle différence entre couverture de lignes et couverture de branches ? Un code couvert à 100 % est-il forcément sans bug ? Justifiez avec un exemple tiré de ce TP.

- Couverture de lignes : mesure le pourcentage de lignes de code exécutées par les tests.
- Couverture de branches : mesure le pourcentage de chemins possibles des conditions (if/else, par exemple) exécutés.
Une couverture de 100 % ne garantit pas l'absence de bugs. Un test peut exécuter toutes les lignes sans vérifier correctement tous les résultats possibles. Dans ce TP, le bug de formatDuration montre qu'un code peut sembler fonctionner, mais produire un résultat incorrect pour certains cas.



## Partie 3 — Tests de composant

**Q8.** Que vérifie `CoursCard.test.tsx` qu'un test unitaire de `formatDuration` ne vérifie pas ? Quel outil simule le navigateur, et dans quel fichier est-il configuré ?

CoursCard.test.tsx vérifie le comportement et l'affichage du composant React : titre, durée, informations du cours et éléments visibles.

Un test unitaire de formatDuration vérifie uniquement le résultat de la fonction. Il ne vérifie pas son intégration dans l'interface.

L'outil utilisé pour simuler le navigateur est jsdom. Sa configuration se trouve dans le fichier de configuration Vitest, généralement vitest.config.ts.



**Q9.** Pourquoi les tests recherchent-ils les éléments par rôle ou par texte (`getByRole`, `getByText`) plutôt que par classe CSS ?


getByRole et getByText recherchent les éléments selon leur rôle et leur contenu visible, comme le ferait un utilisateur ou un outil d'accessibilité.

Les classes CSS peuvent changer lors d'une modification du design. Les tests basés sur les rôles et les textes sont donc plus stables et vérifient davantage le comportement réel de l'interface.





**Q10.** Pourquoi n'y a-t-il pas de test de composant pour `app/page.tsx` ? Quel niveau de test le couvre ?

 Il n'y a pas de test de composant pour app/page.tsx parce que cette page représente l'assemblage global de l'application. Elle est plutôt vérifiée par les tests E2E, qui reproduisent le parcours d'un utilisateur dans le navigateur.


## Partie 4 — Tests d'intégration

**Q11.** Que vérifient les tests d'intégration qui n'est vérifié par aucun test unitaire ou de composant ?

Les tests d'intégration vérifient que plusieurs parties de l'application fonctionnent correctement ensemble : frontend, API, Strapi et échanges HTTP.

Ils vérifient notamment que les données sont correctement récupérées et transmises entre les différents services.


**Q12.** Strapi arrêté : quel résultat et quel message obtenez-vous ? Pourquoi les tests unitaires passent-ils toujours ? À quoi sert `--no-deps` ?


Lorsque Strapi est arrêté, les tests d'intégration échouent parce que les appels HTTP ne peuvent pas atteindre l'API. Dans notre cas, les tests ont échoué avec un délai d'attente de 5 secondes.

Les tests unitaires passent toujours parce qu'ils testent des fonctions isolées sans dépendre de Strapi.

L'option --no-deps permet de lancer un service Docker Compose sans démarrer automatiquement ses dépendances.

**Q13.** Un test d'intégration en échec signale-t-il forcément un bug dans le code ? Citez deux autres causes possibles.

Non, un échec d'intégration ne signifie pas forcément qu'il existe un bug dans le code.

Deux autres causes possibles :
- Un service externe est arrêté ou inaccessible.
- Un problème de réseau, de configuration ou de base de données empêche la communication.

## Partie 5 — Tests de bout en bout

**Q14.** Quel scénario échoue ? Que montrent la capture d'écran et la trace ? Quel lien avec la question Q3 ? Pourquoi échoue-t-il alors que vous avez déjà corrigé le bug en partie 2 ?


Le scénario qui échoue est celui qui rencontre un problème pendant le parcours utilisateur dans le navigateur.

La capture d'écran montre l'état de la page au moment de l'échec. La trace Playwright permet de retrouver les actions réalisées, les erreurs et les requêtes effectuées.

Le lien avec Q3 est que le même problème fonctionnel peut être détecté à deux niveaux : par un test unitaire et par un test E2E.

Même après la correction du bug dans la fonction, un test E2E peut encore échouer si une ancienne version du frontend est utilisée, si le service backend est indisponible ou si un autre problème existe dans le parcours.

**Q15.** Le même bug est détecté par un test unitaire et par un test E2E. Lequel permet de trouver la cause le plus vite ? Lequel prouve que l'utilisateur est touché ?

Le test unitaire permet de trouver la cause plus rapidement, car il teste directement la fonction concernée.



Le test E2E prouve que le problème touche réellement le parcours de l'utilisateur dans l'application.

**Q16.** Complétez le tableau avec vos mesures :

| Niveau | Nombre de tests | Durée d'exécution | Besoin de la stack ? |
|---|---|---|---|
| Unitaire + composant |21 tests réussis dans notre dernière exécution |2,82 s | Non|
| Intégration |4 tests |20,36 s |oui |
| E2E | 4 | 26,09 s| oui|

**Q17.** Qu'est-ce qu'un test « flaky » (instable) ? Quels mécanismes de Playwright utilisés ici (attente automatique, assertions `expect(...).toHaveCount`, etc.) réduisent ce risque ?

Un test flaky est un test instable qui peut réussir ou échouer sans modification du code.

Playwright réduit ce risque grâce à :
- L'attente automatique des éléments avant les interactions.
- Les assertions qui attendent que la condition soit satisfaite.
- expect(...).toHaveCount() qui attend le nombre d'éléments attendu.
- Les mécanismes de vérification de visibilité et d'accessibilité des éléments.


## Partie 6 — SonarQube

**Q18.** Collez votre `sonar-project.properties` complété et expliquez chaque valeur ajoutée. Pourquoi exclut-on `app/**` et `lib/api.ts` du calcul de couverture ?

file de sonar-project.properties

sonar.projectKey=tp3-cours
sonar.projectName=TP3 - Cours DevOps
sonar.sources=app,components,lib
sonar.tests=tests
sonar.javascript.lcov.reportPaths=coverage/lcov.info
sonar.exclusions=node_modules/**,.next/**,coverage/**,playwright-report/**,test-results/**
sonar.coverage.exclusions=app/**,lib/api.ts,components/CoursList.tsx,components/DevOpsLoop.tsx,*.config.*
sonar.sourceEncoding=UTF-8
sonar.qualitygate.wait=true

Explication :
- sonar.projectKey : identifiant unique du projet.
- sonar.projectName : nom affiché dans SonarQube.
- sonar.sources : dossiers contenant le code source.
- sonar.tests : dossier contenant les tests.
- sonar.javascript.lcov.reportPaths : emplacement du rapport de couverture.
- sonar.exclusions : fichiers et dossiers exclus de l'analyse.
- sonar.coverage.exclusions : fichiers exclus du calcul de couverture.
- sonar.sourceEncoding : encodage UTF-8.
- sonar.qualitygate.wait=true : attend le résultat du Quality Gate et retourne une erreur si celui-ci échoue.
On exclut app/** car la page principale est couverte par les tests E2E plutôt que par les tests unitaires. On exclut lib/api.ts car il dépend des appels à l'API externe. Les composants indiqués sont également exclus du calcul de couverture selon les choix du TP


**Q19.** Analyse avec le Quality Gate TP3 : statut du gate et conditions en échec ; nombre de bugs, vulnérabilités, security hotspots et code smells ; couverture ; taux de duplication.

Notre analyse SonarQube a bien été envoyée et traitée. Le résultat obtenu était :
Quality Gate : FAILED
Le scanner a affiché :
ERROR QUALITY GATE STATUS: FAILED




**Q20.** Expliquez la différence entre bug, vulnérabilité, security hotspot et code smell, avec un exemple tiré de votre rapport quand il en existe.

Bug : erreur dans le code pouvant provoquer un mauvais fonctionnement. Exemple : un calcul incorrect.

Vulnérabilité : faiblesse de sécurité pouvant être exploitée par un attaquant. Exemple : un mot de passe écrit directement dans le code.

Security Hotspot : portion de code nécessitant une vérification de sécurité humaine.

Code smell : mauvaise pratique qui rend le code plus difficile à comprendre, maintenir ou faire évoluer.



**Q21.** Ouvrez la vulnérabilité : quelle règle est en cause et quel est le risque ? Expliquez la correction fournie : d'où vient maintenant le mot de passe, et par quel chemin arrive-t-il jusqu'au code ? Pourquoi un secret dans le code est-il dangereux, même dans un dépôt privé ?

La vulnérabilité concerne la gestion d'un secret, notamment un mot de passe écrit directement dans le code.

La correction consiste à ne plus stocker le mot de passe dans le code source, mais à le récupérer depuis une variable d'environnement.

Le chemin est le suivant :
1. Le mot de passe est défini dans le fichier .env.
2. Docker Compose transmet la variable d'environnement au service concerné.
3. L'application récupère cette variable.
4. Le code utilise sa valeur pour se connecter au service.

**Q22.** Quels code smells la correction fournie de `CoursList.tsx` fait-elle disparaître ? Pour chacun : règle, sévérité, explication, et ce qui, dans le nouveau code, règle le problème.


La correction de CoursList.tsx vise à supprimer les mauvaises pratiques détectées par SonarQube.

Pour chaque code smell, il faut relever dans l'onglet Issues :
- La règle SonarQube.
- La sévérité.
- Le problème signalé.
- La modification effectuée dans le nouveau code.
Les corrections peuvent notamment concerner la simplification des conditions, la suppression de code inutile ou la réduction de la complexité. Il faut utiliser les règles réellement affichées dans votre rapport pour répondre précisément.


**Q23.** Où se trouve le code dupliqué ? Comment le supprimer ? (Ne le faites pas, décrivez seulement.)


Le code dupliqué correspond à des portions de code identiques ou très similaires présentes à plusieurs endroits du projet.

Pour supprimer cette duplication, on peut extraire la partie commune dans une fonction ou un composant réutilisable, puis appeler cette fonction aux différents endroits. Cela évite de répéter le même code et facilite sa maintenance.


**Q24.** Qu'est-ce que la dette technique dans SonarQube et comment est-elle exprimée ? Valeur avant et après vos corrections.

La dette technique représente le travail supplémentaire nécessaire pour corriger les mauvaises pratiques et améliorer la qualité du code.

Dans SonarQube, elle est exprimée en temps estimé de correction, par exemple en minutes, heures ou jours.
- Dette avant correction : à relever dans SonarQube.
- Dette après correction : à relever après la nouvelle analyse.


**Q25.** Que signifie « New Code » dans SonarQube ? Quelles conditions du gate TP3 portent sur le nouveau code ? Pourquoi SonarQube recommande-t-il de juger le nouveau code plutôt que le code global ?

New Code désigne le code ajouté ou modifié pendant une période de référence définie dans SonarQube.

Les conditions du Quality Gate TP3 portent sur la qualité du nouveau code, notamment la couverture, les duplications et les nouvelles anomalies.

SonarQube recommande de surveiller le nouveau code pour éviter d'ajouter de nouveaux problèmes, sans exiger de corriger immédiatement toute la dette technique historique du projet.


**Q26.** Avec `sonar.qualitygate.wait=true`, que fait le scanner quand le Quality Gate est rouge ? Pourquoi est-ce utile dans un pipeline CI ?

Avec sonar.qualitygate.wait=true, le scanner attend que SonarQube termine l'analyse et vérifie le Quality Gate.

Si le Quality Gate est rouge, le scanner retourne une erreur et le code de sortie est non nul.

C'est utile dans un pipeline CI parce que l'échec du Quality Gate peut bloquer automatiquement la validation ou le merge d'une Pull Request.

## Partie 7 — Synthèse

**Q27.** Proposez l'ordre des étapes d'un pipeline CI pour ce projet (build, tests unitaires, intégration, E2E, analyse Sonar…) et justifiez-le. Quelles étapes doivent bloquer le merge d'une pull request ?


L'ordre proposé pour le pipeline CI est :
1. Checkout : récupérer le code source depuis Git.
2. Installation des dépendances : préparer l'environnement.
3. Build : vérifier que l'application peut être compilée.
4. Tests unitaires et de composants : vérifier rapidement les fonctions et les composants.
5. Tests d'intégration : vérifier les échanges entre les différents services.
6. Tests E2E : vérifier le parcours réel de l'utilisateur.
7. Analyse SonarQube : contrôler la qualité, la sécurité, la couverture et les duplications.
8. Validation et merge : autoriser le merge uniquement si les contrôles sont réussis.


Les tests unitaires sont placés avant les tests d'intégration et E2E parce qu'ils sont plus rapides et permettent de détecter les erreurs tôt.

Les étapes qui doivent bloquer le merge sont :
- Le build en cas d'échec.
- Les tests unitaires et composants en cas d'échec.
- Les tests d'intégration en cas d'échec.
- Les tests E2E en cas d'échec.
- Le Quality Gate SonarQube si son statut est FAILED.