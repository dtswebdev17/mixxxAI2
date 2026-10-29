# Branche API — état des lieux (types C++ exposés au QML)

Lecture seule du dépôt à `ee17a272`. Aucun code n'a été modifié, compilé ou
testé dans ce commit. Interprétation retenue pour « API » : les types C++
enregistrés dans le module QML `Mixxx` (`src/qml/`). À confirmer.

## Inventaire

- `src/qml/` contient 83 fichiers ; 33 en-têtes déclarent des types QML
  (`QML_NAMED_ELEMENT` / `QML_ELEMENT`), dont 12 déclarent des singletons.
- Types exposés : Application, Battery, ChainPresetModel, Config,
  ControlProxy, ControllerDevice, ControllerManager, ControllerScreen, Core,
  EffectSlotParametersModel, EffectSlotProxy, EffectUnitProxy,
  EffectsManager, InputConnection, InputDevice, KeyUtils, LegacyLibraryItem,
  Library, LibraryAbstractSource, LibraryAllTrackSource, LibraryScanner,
  LibrarySource, LibrarySourceTree, LibraryTrackListModel, OutputConnection,
  OutputDevice, Player, PlayerManager, PreferencesDialog, Recording,
  SettingGroup, SettingParameter, SettingParameterManager,
  SkinControlCreator, SoundDevice, SoundManager, SpinnyPosition, Track,
  TrackListColumn, VinylSignalQuality, VisibleEffectsModel, WaveformDisplay,
  WaveformMark, WaveformMarkRange, WaveformOverview, WaveformRenderer*
  (Beat, EndOfTrack, Filtered, HSV, Mark, MarkRange, Preroll, RGB, Simple,
  Stem), WaveformUntilMark.
- Tests existants côté QML : `qmlcontrolproxytest`, `qmleffectsproxytest`,
  `qmllibraryproxytest`, `qmlskincontrolcreatortest`, `qmlspinnypositiontest`,
  `qmlwaveformoverviewtest`, `qmlkeyutilstest`, `qmldurationformattertest`,
  `qmlstartupsmoketest`, `themeqml_test`.

## Constats issus de sessions de test réelles (Linux Mint 22.3, Qt 6.4.2)

1. **`Mixxx.Application` mal résolu avec Qt 6.4.2** : au démarrage, des
   avertissements indiquent `Property 'menuShortcut' of object
   QQuickApplication ... is not a function` et `Unable to assign [undefined]
   to bool` pour `supportsGlobalMenuBar` / `keyboardShortcutsEnabled`
   (`ApplicationMenuActions.qml:57,310,346,357,368`, `MainMenuBar.qml:103`,
   `MainWindow.qml:108`). Le singleton C++ est `QmlApplicationProxy`
   (`QML_NAMED_ELEMENT(Application)`, `QML_SINGLETON`) ; QML obtient à la place
   l'objet `Application` de Qt. Hypothèse non vérifiée : conflit de nom avec le
   type `Application` de `QtQuick` sur cette version de Qt.
2. **Contrôle `[Skin],show_intro_outro_cues` absent** : avertissement
   `QmlControlProxy: Requested CO ... does not exist, resetting...` en boucle.
   Il est demandé par `res/qml/Mixxx/Controls/WaveformOverviewMarkerLayer.qml`
   et par `res/skins/LateNightQML/Toolbar/Toolbar.qml` ; il n'est défini que
   par les skins XML (Deere). À clarifier : qui doit le créer pour un skin QML ?
3. **Hypothèse d'une interface à widgets** : `CrateFeature::slotCreateCrate`
   déréférençait `m_pSidebarWidget` (nul en QML) et faisait planter Mixxx
   (SIGSEGV). Correctif de deux lignes sur la branche `QML` (`ff5bc459`).
   `LibraryControl` utilise aussi `m_pSidebarWidget` (lignes ~1102, ~1133) ;
   ces accès dépendent du contexte de focus et n'ont pas été vérifiés.

## Pistes de travail proposées (à valider)

- [ ] Reproduire le point 1 sur Qt 6.4.2 puis sur une version plus récente.
- [ ] Décider qui crée `[Skin],show_intro_outro_cues` pour les skins QML.
- [ ] Rechercher d'autres déréférencements de widgets de l'ancienne interface
      appelés depuis le QML (`src/qml/*proxy.cpp`).
- [ ] Ajouter des tests pour tout type modifié (voir les tests ci-dessus).
