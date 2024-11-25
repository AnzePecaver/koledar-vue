# Preprost koledar v Vue 3

Preprost koledar v obliki spletne aplikacije, zgrajene z uporabo Vue 3. Uporabnikom omogoča ogled dni za izbran mesec in leto. Nedelje in prazniki so posebej označeni, pri čemer so podatki o praznikih naloženi iz tekstovne datoteke.

## Funkcionalnosti
- Ogled koledarja za katerikoli mesec in leto. Dnevi so razporejeni v vrstice po tednih.
- Alternativno je omogočen vpis poljubnega datuma za izbiro meseca in leta.
- Nedelje so samodejno označene z rdečo barvo besedila.
- Prazniki se preberejo iz tekstovne datoteke `prazniki.txt` in so označeni s svetlo vijoličnim krogcem in napisom.
- Odziven in sodoben dizajn, ki se prilagaja različnim velikostim zaslona
- Možnost zapiranja (skrivanja) in odpiranja koledarja

## Namestitev in uporaba

### Zagon aplikacije

Zgrajena različica aplikacije se nahaja v direktoriju `dist`. Za strežbo datotek je potreben HTTP strežnik, kot na primer z uporabo Pythona:

   ```sh
   python -m http.server
   ```

Za uporabo aplikacije odprite brskalnik in pojdite na `http://localhost:8000`.

### Gradnja aplikacije iz izvorne kode

Za gradnjo aplikacije iz izvorne kode sledite tem korakom:

1. Namestite odvisnosti:
   ```sh
   npm install
   ```

2. Zgradite aplikacijo:
   ```sh
   npm run build
   ```

3. Zgrajena različica aplikacije bo v direktoriju `dist`.

4. Za zagon zgrajene različice sledite korakom v razdelku "Zagon aplikacije" zgoraj.

### Način razvoja

Če želite zagnati aplikacijo v načinu razvoja, uporabite naslednji ukaz:

```sh
npm run dev
```

Aplikacija bo na voljo na izpisanem naslovu.

## Konfiguracija
- Prazniki se nalagajo iz tekstovne datoteke `prazniki.txt`, ki se nahaja v direktoriju `dist`. Podatki so podani v formatu `YYYY-MM-DD` in so ločeni z vejico. Če je praznik ponavljajoč, je namesto leta podan znak 'P'. To datoteko lahko uredite, da dodate svoje praznike.

## Uporabljene tehnologije
- Vue 3
