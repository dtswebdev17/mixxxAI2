# Branche MIDI — état des lieux (contrôleurs)

Lecture seule du dépôt à `ee17a272`. Aucun code n'a été modifié, compilé ou
testé dans ce commit. Interprétation retenue pour « MIDI » : la prise en charge
des contrôleurs (MIDI, HID, bulk) et leur configuration. À confirmer.

## Inventaire

- **Code** (`src/controllers/`) : `controller`, `controllermanager`,
  `controllerenumerator`, `legacycontrollermapping*` (+ file handler),
  `legacycontrollersettings*`, `softtakeover`, `learning*`, sous-dossiers
  `midi`, `hid`, `bulk`, `scripting`, `rendering`, `keyboard`, `delegates`.
- **Préférences (widgets)** : `dlgprefcontroller(s)`, `dlgcontrollerlearning`.
- **Préférences (QML)** : `res/qml/Settings/Controller.qml`,
  `ControllerCard.qml`, `ControllerSettings.qml`, `ControllerScreens.qml`,
  `ControllerMappingLink.qml`.
- **Mappings** (`res/controllers/`) : 328 entrées, dont 144 `*.midi.xml`,
  18 `*.hid.xml` et 152 scripts `*.js`, plus des scripts communs
  (`common-controller-scripts.js`, `common-hid-packet-parser.js`,
  `common-bulk-midi.js`) et des écrans QML (`TraktorKontrolS4MK3Screens.qml`,
  `DummyDeviceDefaultScreen.qml`).
- **Tests** : `midicontrollertest`, `portmidicontroller_test`,
  `portmidienumeratortest`, `controller_mapping_validation_test`,
  `controller_mapping_file_handler_test`, `controller_mapping_settings_test`,
  `controllerscriptenginelegacy_test`, `controllerrenderingengine_test`,
  `controller_hid_reportdescriptor_test`, `controlobjectscripttest`.

## Observations

- Aucun matériel MIDI n'a été testé dans cette session : ni détection, ni
  mapping, ni boucle de latence.
- Le document de cadrage demande de préserver les « interactions MIDI » dans
  la première PR des panneaux ; elles passent par `ControlProxy`, qui n'est pas
  touché par la mise en page.

## Pistes de travail proposées (à valider)

- [ ] Définir l'objectif de la branche (nouveau mapping ? réglages QML ?
      apprentissage MIDI ?) avant tout code.
- [ ] Définir une procédure de test avec un contrôleur réel ou le
      `dummy_hid_device` présent dans `tools/`.
- [ ] Vérifier que les contrôles `[Skin]` créés par le QML n'entrent pas en
      conflit avec les mappings existants.
