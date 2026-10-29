# Branche FX — état des lieux (effets)

Lecture seule du dépôt à `ee17a272`. Aucun code n'a été modifié, compilé ou
testé dans ce commit. Interprétation retenue pour « FX » : les effets audio
et leur interface QML. À confirmer.

## Inventaire

- **Moteur** (`src/effects/`) : `EffectsManager`, `EffectChain`, `EffectSlot`,
  `EffectParameter` (+ slots bouton/potentiomètre), `VisibleEffectsList`,
  `EffectsMessenger`, dossiers `backends`, `chains`, `presets`.
- **Exposition QML** (`src/qml/`) : `qmleffectsmanagerproxy`,
  `qmleffectunitproxy`, `qmleffectslotproxy`, `qmleffectslotparametersmodel`,
  `qmlvisibleeffectsmodel` (dix `Q_PROPERTY` sur les proxys d'unité et de
  gestionnaire).
- **QML générique** (`res/qml/Effects/`) : `EffectUnit`, `EffectSlot`,
  `EffectSelector`, `EffectFocusButton`, `EffectAssignmentButtons`,
  `EffectChainPresetPopup`, `EffectSlotPresetMenu`, `EffectPresetMenuItem`,
  `EffectPresetMenuSeparator`.
- **LateNightQML** (`res/skins/LateNightQML/Effects/`) : `EffectsRack`,
  `EffectUnit`, `EffectSlot`, `EffectParameter`, `EffectSelector`,
  `EffectFocusButton`, `EffectControlButton`.
- **Tests** : `nativeeffects_test`, `engineeffectsdelay_test`,
  `qmleffectsproxytest`.

## Observations

- Le skin générique affiche les effets via `EffectUnit` (unités 1 à 4) ; dans
  `LateNightQML`, la section FX est un `Item` (`effectsSection`) dont la hauteur
  vaut `effectsRack.implicitHeight` ou 0 : afficher/masquer, pas
  redimensionnable (voir `docs/latenightqml/audit-phase1.md` sur la branche
  `LATENIGHTQML`).
- Deux jeux de composants parallèles (`res/qml/Effects` et
  `LateNightQML/Effects`) : risque de divergence si l'un évolue seul.

## Pistes de travail proposées (à valider)

- [ ] Comparer les deux jeux de composants et lister les écarts de fonction.
- [ ] Couvrir par un test QML le chargement d'un `EffectUnit` complet.
- [ ] Faire de la section FX un panneau redimensionnable (dépend de la
      phase 1 de `LATENIGHTQML`).
