# Rappels de syntaxes et d'utilisation de module pour Ansible

## Boucles

### Boucle avec items(objets)

Pour utiliser une boucle avec des items(objets) il faut au préalable définir la liste d'objets, il faut aussi définir l'endroit ou on utilise l'item.
```yaml
---
- name: Utilisation simple d'une boucle avec des items
  debug:
    msg: "{{ item }} est un véhicule"
  with_items:
  - voiture
  - bus
  - velo
```
Un item peut contenir plusieurs valeurs sous le format `clé: valeur` et on peut appeler la valeur en fonction de la clé.
```yaml
- name: Utilisation plus complexe d'une boucle avec des items
  debug:
    msg: "{{ item.nom }} est un véhicule qui à {{ item.nbroues }} roues"
  with_items:
  - { nom: voiture, nbroues: 4 }
  - { nom: bus, nbroues: 6 }
  - { nom: vélo, nbroues: 2 }
```
On peut coupler la boucle with_items à une condition when.

```yaml
- name: Condition une
  debug:
    msg: "{{ item.nom }} est un véhicule motorisé à {{ item.nbroues }} roues"
  when: {{ item.moteur }} == true
  with_items:
  - { nom: voiture, nbroues: 4, moteur: true }
  - { nom: bus, nbroues: 6, moteur: true }
  - { nom: vélo, nbroues: 2, moteur: false }

- name: Condition deux
  debug:
    msg: "{{ item.nom }} est un véhicule non motorisé à {{ item.nbroues }} roues"
  when: {{ item.moteur }} == false
  with_items:
  - { nom: voiture, nbroues: 4, moteur: true }
  - { nom: bus, nbroues: 6, moteur: true }
  - { nom: vélo, nbroues: 2, moteur: false }
```