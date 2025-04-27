# 📋 Procédures CRUD PostgreSQL

## 📑 Sommaire

- [🔄 Procédures CRUD](#-procedure-crud)
  - [➕ Procédure d'ajout d'un acteur](#-procedure-dajout-dun-acteur)
  - [🔍 Procédures de lecture](#-procedure-de-lecture-dun-acteur-par-son-id)
    - [👤 Lecture d'un acteur par ID](#-lecture-dun-acteur)
    - [👶 Lecture des acteurs de moins de 30 ans](#-lecture-multiple-acteur-de-moins-de-trente-ans)
  - [🔄 Procédures de mise à jour](#-mise-a-jour-dun-acteur)
    - [📝 Mise à jour complète d'un acteur](#-mise-à-jour-de-toutes-les-données-dun-acteur)
    - [✏️ Mise à jour du prénom uniquement](#-mise-à-jour-du-prénom-de-lacteur)
  - [🗑️ Procédure de suppression d'un acteur](#-supprimer-un-acteur)

## 🔄 Procedure CRUD

### ➕ Procedure d'ajout d'un acteur

```SQL
CREATE PROCEDURE add_actor(
IN p_actor_id UUID,
IN p_firstname VARCHAR,
IN p_lastname VARCHAR,
IN p_birthdate DATE,
IN p_character_id UUID,
IN p_character_name VARCHAR,
IN p_character_type VARCHAR,
IN p_movie_id UUID
)
AS $$
BEGIN
INSERT INTO actor (actor_id, actor_firstname, actor_lastname, actor_birthdate)
VALUES (p_actor_id, p_firstname, p_lastname, p_birthdate);
INSERT INTO character (character_id, character_name, character_type)
VALUES (p_character_id, p_character_name, p_character_type);
INSERT INTO acting (actor_id,character_id)
VALUES (p_actor_id, p_character_id);
INSERT INTO movie_characters (movie_id, character_id)
VALUES (p_movie_id, p_character_id);
END;
$$ LANGUAGE plpgsql;
```

### 🔍 Procedure de lecture d'un acteur par son id

#### 👤 Lecture d'un acteur

```sql
CREATE PROCEDURE read_actor_by_id (IN p_actor_id UUID)
AS $$
BEGIN
SELECT * FROM actor
WHERE actor_id = p_actor_id;
END;
$$ LANGUAGE plpgsql;
```

#### 👶 Lecture multiple acteur de moins de trente ans

```sql
CREATE PROCEDURE read_young_actor ( )
AS $$
BEGIN
SELECT * FROM actor
WHERE EXTRACT(YEAR FROM AGE(current_date, actor_birthdate)) < 30;
END;
$$ LANGUAGE plpgsql;
```

### 🔄 Mise a jour d'un acteur

#### 📝 Mise à jour de toutes les données d'un acteur

```sql
CREATE PROCEDURE update_actor(
IN p_actor_id UUID,
IN p_firstname VARCHAR,
IN p_lastname VARCHAR,
IN p_birthdate DATE
)
AS $$
BEGIN
UPDATE actor
SET actor_firstname = p_firstname,
 actor_lastname = p_lastname,
 actor_birthdate = p_birthdate
WHERE actor_id = p_actor_id;
END;
$$ LANGUAGE plpgsql;
```

#### ✏️ Mise à jour du prénom de l'acteur

```sql
CREATE PROCEDURE update_actor_firstname(
IN p_actor_id UUID,
IN p_firstname VARCHAR
)
AS $$
BEGIN
UPDATE actor
SET actor_firstname = p_firstname
WHERE actor_id = p_actor_id;
END;
$$ LANGUAGE plpgsql;
```

### 🗑️ Supprimer un acteur

```sql
CREATE PROCEDURE delete_actor (IN p_actor_id UUID)
AS $$
BEGIN
DELETE FROM acting
WHERE actor_id = p_actor_id;
DELETE FROM actor
WHERE actor_id = p_actor_id;
END;
$$ LANGUAGE plpgsql;
```
