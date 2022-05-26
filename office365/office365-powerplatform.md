# Power Apps Tips, tricks and quircks

In de Nederlandstalige versie van Power Apps wordt de getalnotatie 

``Text(ThisItem.Price; "$ ##.00")`` automatisch omgezet in ``Text(ThisItem.Price; "[$-nl]$ ##.00")`` waardoor het getal verkeerd in de app wordt weergegeven. De oplossing hiervoor is aan te geven dat de notatie Engelstalig dient te zijn. De juiste formule derhalve is:


```
Text(ThisItem.Price; "[$-en-US]$ ##.00")
```
