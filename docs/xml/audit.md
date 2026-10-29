# Branche XML — état des lieux (skins et mappings XML)

Lecture seule du dépôt à `ee17a272`. Aucun code n'a été modifié, compilé ou
testé dans ce commit. Interprétation retenue pour « XML » : les skins XML
historiques et les fichiers de mapping XML. À confirmer.

## Skins (`res/skins/`)

| Skin | `skin.xml` | Fichiers `.xml` | Fichiers `.qml` |
|---|---|---|---|
| Deere | oui | 107 | 0 |
| Deere (64 Samplers) | oui | 3 | 0 |
| LateNight | oui | 110 | 0 |
| LateNight (64 Samplers) | oui | 3 | 0 |
| Shade | oui | 33 | 0 |
| Tango | oui | 118 | 0 |
| Tango (64 Samplers) | oui | 3 | 0 |
| LateNightQML | non | 0 | 96 |

- Chargement : `src/skin/legacy/` (XML) et `src/skin/qml/` (QML), via
  `skinloader`.
- Schéma : `res/schema.xml` (aucun `.xsd` / `.dtd` trouvé dans `res/`).
- **Mappings de contrôleurs** : `res/controllers/*.midi.xml` (144) et
  `*.hid.xml` (18), lus par `legacycontrollermappingfilehandler`.
- **Tests** : `skincontext_test`, `controller_mapping_validation_test`,
  `controller_mapping_file_handler_test`, `qmlskincontrolcreatortest`.

## Observations

- `LateNightQML` n'a pas de `skin.xml` ; il se déclare par `skin.ini`.
- Les contrôles `[Skin],…` sont créés par les skins XML (`skin_settings.xml`
  de Deere, par exemple) ; les skins QML doivent les créer eux-mêmes
  (`SkinControlCreator`). Voir `docs/api/audit.md` (branche `API`).

## Pistes de travail proposées (à valider)

- [ ] Définir l'objectif (validation de schéma ? migration XML vers QML ?
      correspondance des contrôles `[Skin]` ?).
- [ ] Lister les contrôles `[Skin]` définis par chaque skin XML et ceux
      attendus par `LateNightQML`.
- [ ] Ajouter un test de validation des mappings XML si l'objectif est la
      qualité des fichiers.
