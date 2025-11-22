--todo-- 1.Masala
create table roles
(
id       serial primary key,
roleName varchar unique not null
);
drop table roles;
truncate table roles;
select *
from roles;
insert into roles(roleName)
values ('Mentor');
insert into roles (roleName)
values ('Student');
create table users
(
id       serial primary key,
fullName varchar unique check ( length(fullName) >= 5),
phone    varchar unique check ( length(fullName) >= 9),
role_id  integer,
foreign key (role_id) references users
);
drop table users;
insert into users (fullName, phone) values ('Babara Shellcross', '562357538');
insert into users (fullName, phone) values ('Patten Seacroft', '383533646');
insert into users (fullName, phone) values ('Devonne Gookey', '385239342');
insert into users (fullName, phone) values ('Steven O''Lenechan', '264060091');
insert into users (fullName, phone) values ('Bondon Cockman', '345983384');
insert into users (fullName, phone) values ('Cacilie Cuncarr', '707881669');
insert into users (fullName, phone) values ('Tate Blackmoor', '167111814');
insert into users (fullName, phone) values ('Dulcy McElhinney', '785469944');
insert into users (fullName, phone) values ('Selestina Kew', '339963822');
insert into users (fullName, phone) values ('Pamelina Fillingham', '748362500');
insert into users (fullName, phone) values ('Roddy McGovern', '522375352');
insert into users (fullName, phone) values ('Goldia Chapell', '989264652');
insert into users (fullName, phone) values ('Christel Grigorkin', '856739117');
insert into users (fullName, phone) values ('Estele Stodart', '467904322');
insert into users (fullName, phone) values ('Earlie Gayther', '265319299');
insert into users (fullName, phone) values ('Thain Linck', '332938228');
insert into users (fullName, phone) values ('Abdel Tremoulet', '568591116');
insert into users (fullName, phone) values ('Harmony Rabbage', '928266429');
insert into users (fullName, phone) values ('Jeane Wyllcock', '858242935');
insert into users (fullName, phone) values ('Marty Rickertsen', '748829724');
insert into users (fullName, phone) values ('Perry Thompsett', '435490283');
insert into users (fullName, phone) values ('Zollie Andreu', '538552831');
insert into users (fullName, phone) values ('Yvor Battill', '995274606');
insert into users (fullName, phone) values ('Gracie Astlett', '958432642');
insert into users (fullName, phone) values ('Chilton McQuarrie', '971741511');
insert into users (fullName, phone) values ('Bertha Witnall', '321769740');
insert into users (fullName, phone) values ('Kerwin Bremeyer', '938862073');
insert into users (fullName, phone) values ('Barrett Aguilar', '386884072');
insert into users (fullName, phone) values ('Marshall Cadwell', '508709712');
insert into users (fullName, phone) values ('Judi Tuffs', '293700851');
insert into users (fullName, phone) values ('Delia Purdon', '855204172');
insert into users (fullName, phone) values ('Trueman Lumbers', '146809910');
insert into users (fullName, phone) values ('Erena Eltringham', '174669884');
insert into users (fullName, phone) values ('Reeta Laughnan', '322073503');
insert into users (fullName, phone) values ('Zelma Wixon', '231262840');
insert into users (fullName, phone) values ('Almeria Skitterel', '128787632');
insert into users (fullName, phone) values ('Saleem Sanbrook', '325448801');
insert into users (fullName, phone) values ('Clarey Checketts', '916995536');
insert into users (fullName, phone) values ('Noellyn Petrelli', '143863278');
insert into users (fullName, phone) values ('Stewart Chantrell', '792815921');
insert into users (fullName, phone) values ('Cart Boffin', '961915658');
insert into users (fullName, phone) values ('Kale Hawkswood', '386849218');
insert into users (fullName, phone) values ('Laureen Allin', '607254159');
insert into users (fullName, phone) values ('Gabriello Bein', '895690392');
insert into users (fullName, phone) values ('Giffy Clousley', '561086432');
insert into users (fullName, phone) values ('Joanie Duplain', '697824082');
insert into users (fullName, phone) values ('Sallyanne Gallandre', '223215144');
insert into users (fullName, phone) values ('Evelyn Spottiswood', '393878897');
insert into users (fullName, phone) values ('Anjanette Edwinson', '979779679');
insert into users (fullName, phone) values ('Esme Hanscome', '106482182');

create table groups
(
id        serial primary key,
groupName varchar check (length(groupName) >= 3),
mentor_id integer,
creatAt  date    not null default now(),
foreign key (mentor_id) references groups
);

insert into groups (groupName, creatAt) values ('Sipes, Roob and Beatty', '8/4/2025');
insert into groups (groupName, creatAt) values ('Dietrich-Friesen', '10/14/2025');
insert into groups (groupName, creatAt) values ('Kling, Volkman and Erdman', '12/28/2024');
insert into groups (groupName, creatAt) values ('Ratke LLC', '2/2/2025');
insert into groups (groupName, creatAt) values ('Balistreri, Champlin and McDermott', '2/14/2025');
insert into groups (groupName, creatAt) values ('Schoen-Kemmer', '5/30/2025');
insert into groups (groupName, creatAt) values ('Roberts LLC', '2/7/2025');
create table students
(
id       serial primary key,
user_id  integer,
foreign key (user_id) references students,
group_id integer,
foreign key (group_id) references groups,
creatAt  date    not null default now(),
active   boolean not null default false
);
drop table students;


--todo-- 2.Masala --(Standart viewga doir)
-- Materialized view
CREATE OR REPLACE VIEW st_view AS
SELECT
id as student_id,
user_id,
creatAt as student_creatAt,
group_id as groupName,
active
FROM students
ORDER BY id;


--todo-- 3. Masala --(Materialized view ga doir)
-- Materialized view
CREATE MATERIALIZED VIEW mw_users AS
SELECT id,
fullName,
phone,
role_id
FROM users
ORDER BY id;

--todo--4.Masala -->(Function ga doir)
--  Foydalanuvchi qo'shish funktsiyasi
CREATE OR REPLACE FUNCTION add_user(
user_fullName VARCHAR(100),
user_phone VARCHAR(255)
)
RETURNS INTEGER AS
$$
DECLARE
new_user_id INTEGER;
BEGIN
INSERT INTO users (fullName, phone, role_id)
VALUES (fullName,phone,role_id)
RETURNING id INTO new_user_id;

    RETURN new_user_id;
END;
$$ LANGUAGE plpgsql;

-- Funksiyani ishlatish
SELECT add_user('John Smith', '99 999 99 99');

--todo-- 5. Masala -->(Prodseduraga oid)
-- 1. Foydalanuvchi qo'shish prosedurasi
CREATE OR REPLACE PROCEDURE add_user_procedure(
user_name VARCHAR(100),
user_phone VARCHAR(255)
)
LANGUAGE plpgsql
AS
$$
BEGIN
INSERT INTO users (fullName, phone)
VALUES (user_name, user_phone);

    RAISE NOTICE 'Foydalanuvchi muvaffaqiyatli qo''shildi: %', user_name;
EXCEPTION
WHEN unique_violation THEN
RAISE EXCEPTION 'Bu nomer allaqachon mavjud: %', user_phone;
WHEN OTHERS THEN
RAISE EXCEPTION 'Xatolik yuz berdi: %', SQLERRM;
END;
$$;

-- Prosedurani ishlatish
CALL add_user_procedure('Jamshid Jumayev', '60 962 92 82');
select *
from users;










--todo--Savollarga Javoblar
--1.DB ?
-- Javob: Tartiblangan ma'lumotlarning tizimli to'plami.
--2. RDBMS ?
--Javob: Realtion DBMS bo'lib , database ni boshqarishimiz uchun ishlatiladigan Dasturlash tili. Unda delete, insert, updete....
--3. RDBMS va DBMS ning farqi.
--Javob: RDBMS bu DBMS ning takomillashgan versiyasi . Asosi DBMS hisoblanadi
--4. SQL nima? va Nima uchun ishlatiladi?
--Javob: SQL bu aynan ma'lumoylar ba'zasi bn ishlash uchun maxsus yaratilgan dasturlash tili.
--U java kodlarini DB da yozish imkonini beradi
--5. primary key va foreign key nima va bir-biridan farqi nima?
--Javob: PostgreSQL da primary key constrains jadvaldagi har bir qatorni yagona tarzda identifikatsiya qilish un
--ishlatiladi(not null va unique).
-- Foreign key- jadval ustunidagi qiymatlar boshqa jadval ning Primary key yoki unique ustunidagi qiymatlar mos kelishini taminlaydi
-- primary key not null va unique likni ta'minlab beradi, foreign key esa jadvallarni birlashtirishda ularning mosligini ta'minlab beradi.
--6. PL/pgSQL nima?
--Javob: Bu PostgreSQL ma'lumotlar ba'zasi boshqaruv tizimida ishlatiladigan prodsedual dasturlash tili.
--[if-else], [Loop, for, while]...
--dastlab PostgreSQL ning 6.4 version da qo'shilgan, 9.0 dan boshlab default mavjud.
--7.Assert statment nima va nima uchun ishlatiladi?
--Javob: Assert statment bu shartning to'g'ri yoki xatoligini tekshiradi va agar xato bo'lsa
-- exeption qaytaradi [PostgreSQL ning 9.5 version]ida qo'shilgan
--8.Trigger nima?
--Javob: Triggerlar PostgreSQL da jadvalda ma'lum bir hodisa(EVENT) sodir bo'lganda avtomatik ravishda chaqiriladigan
--funksiyadir.
--9. Transaction and ASID ?
--Javob: Tranzaksiya DB da 1 yoki bir nechta operatsiyalarni bitta operatsiya sifatida amalga oshirilishiga aytiladi
-- Agar birorta apetatsiya xato bo'lsa barcha operatsiyalar orqaga qaytariladi.
-- ASID esa Tranzaksiyalarning asosiy xususiyatlari hisoblanadi
--*Atomicity
--*Consistency
--*Isolatsion
--*Durability
--10. Indexing va uning vazifasi
--Javob: DB da so'rovlar bn ishlashda ishlash tezligini oshirishga mo'ljallangan.