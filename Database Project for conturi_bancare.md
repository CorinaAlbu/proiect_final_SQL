# Creare si utilizare baza de date *Conturi bancare*


*&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.*

**Application under test**: SQL- creation and use of data base "Conturi_bancare"

**Tools used**: MySQL Workbench

**Database description**: creation of a data base of bank clients, containing their personal data and data on the products owned by each of them. It is mandatory that each client owns at least one current account while the rest of the products (savings, loans) are optional, depending on each client. For this purpose 5 tables have been created:
- Table **client** with corresponding data: ID_client, nume, prenume, data_nasterii, nr_telefon, adresa_email, adresa_domiciliu, adresa corespondenta, stare civila. 
- Table **produse_cont_curent** with columns: ID, ID_client, cont_crt_RON_sold, data_desch_cont, comision_lunar, suma_OD; each client has at least one current account.
- Table **produse_economisire** with columns: ID, ID_client, produs, data_activarii, suma_initiala, suma_curenta. Not all clients own saving products.
- Table **produse_creditare** with columns: ID, ID_Client, suma_acordata_NP, data_acordarii_NP, perioada_NP_luni, suma_outst_NP, suma_acordata_IMOB, data_acordarii_IMOB, perioada_IMOB_luni, suma_outst_IMOB. Clients may have personal loans (marked on the columns containing NP) and/or mortgage loans (marked on the columns containing IMOB). Not all clients have active loans.
- Table **chestionar_client_opinion** with columns: ID, ID_client, data_chestionar, nota, feedback_client. Not all clients have provided feedback.

### 1. Database Schema

The following database schema has been generated through Reverse Engineer and it contains all the tables and the relationships between them.

The tables are connected in the following way:

-- **clienti** is connected with **produse_cont_curent** through a **one-to-one** relationship which was implemented through ** clienti.ID_client** as a primary key and **produse_cont_curent.ID_client** as a foreign key
-- **clienti** is connected with **produse_creditare** through a **one-to-one** relationship which was implemented through ** clienti.ID_client** as a primary key and **produse_creditare.ID_client** as a foreign key
-- **clienti** is connected with **produse_economisire** through a **one-to-one** relationship which was implemented through ** clienti.ID_client** as a primary key and **produse_economisire.ID_client** as a foreign key
-- **clienti** is connected with **chestionar_client_opinion** through a **one-to-one** relationship which was implemented through ** clienti.ID_client** as a primary key and **chestionar_client_opinion.ID_client** as a foreign key

### 2.Database Queries

##### DDL (Data Definition Language)

The following instructions were written in the scope of CREATING the structure of the database (**CREATE** INSTRUCTIONS)

>create database conturi_bancare;

>use conturi_bancare;

>create table clienti (
ID_client int primary key auto_increment not null, 
Nume varchar (15) not null,
Prenume varchar (25) not null,
Data_nasterii date not null,
Nr_telefon varchar (20) not null,
Adresa_email varchar (30) not null,
Adresa_domiciliu varchar (50) not null,
Adresa_corespondenta varchar (50) not null,
Stare_civila varchar (15)
);

>create table produse_cont_curent (
ID int primary key auto_increment not null,
ID_client int not null,
Cont_crt_RON_sold decimal (6,2) not null,
Data_desch_cont date not null,
Comision_lunar int not null,
Suma_OD decimal (6,2),
foreign key (ID_client) references clienti (ID_client)
);

>create table produse_creditare (
ID int primary key auto_increment not null,
ID_Client int not null,
Suma_acordata_NP decimal (7,2),
Data_acordarii_NP date,
Perioada_NP_luni int,
Suma_OUTST_NP decimal (7,2),
Suma_acordata_IMOB decimal (7,2),
Data_acordarii_IMOB date,
Perioada_IMOB_luni int,
Suma_OUTST_IMOB decimal (7,2)
);

>create table produse_economisire (
ID int primary key auto_increment not null,
ID_Client int not null,
Produs varchar (25),
Data_activarii date,
suma_initiala decimal (7,2),
suma_curenta decimal (7,2),
foreign key (ID_client) references clienti (ID_client)
);

>create table chestionar_client_opinion (
ID int primary key auto_increment not null,
ID_Client int not null,
Data_chestionar date not null,
Nota int not null,
Feedback_client varchar (250),
foreign key (ID_Client) references clienti (ID_client)
);

After the database and the tables have been created, a few **ALTER** instructions were written in order to update the structure of the database, and one **DROP** instruction in order to remove one column, as described below:

-> a foreign key was added to table produse_creditare:
>alter table produse_creditare
add foreign key (ID_client) references clienti (ID_client);

-> the format of columns containing sums was altered in order to accommodate 8 digit sums with 2 decimal digits (previously the format was 6 digit sums with 2 decimal digits):

>alter table produse_creditare
modify column Suma_OUTST_NP decimal (10,2),
modify column Suma_OUTST_IMOB decimal (10,2);

>alter table produse_creditare
modify column Suma_acordata_NP decimal (10,2),
modify column Suma_acordata_IMOB decimal (10,2);

>alter table produse_economisire
add column interest decimal (4,2);

>alter table produse_economisire
drop column interest;


##### DML (Data Manipulation Language)

In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase.

Below you can find all the insert instructions that were created in the scope of this project:

>insert into clienti 
values
(2, "Trif", "Marius", "1988-10-17", "745231225", "trif.marius@gmail.com", "Timisoara str. Republicii nr. 7 ap. 3", "Timisoara Calea Aradului nr. 10", "casatorit"),
(3, "Trif", "Angela", "1985-12-31", "721986525", "angelat1950@yahoo.com", "Arad Bd Pacii nr. 5, sc. A, ap. 12", "Arad Bd Pacii nr. 5 sc. A ap. 12", "casatorit"),
(4, "Berlovan", "Tatiana", "1985-01-25", "744528623", "berlovan_tat@yahoo.com", "Cluj-Napoca, str. Bucegi nr. 4", "Cluj-Napoca, str. Bucegi nr. 4", "divortat"),
(5, "Cret", "Andrei", "2001-10-05", "748521521", "ndrei1980@gmail.com", "Oradea, pta. Vulturul Negru nr. 2", "Oradea, Calea Aviatorilor, bl. 1, et. 2, ap. 15", "necasatorit"),
(6, "Bostan", "Alexandra", "1999-05-17", "799256326", "bostan.ale@gmail.com", "Sibiu, pta. Sfatului nr. 14", "Sibiu, pta. Sfatului nr. 14", "necasatorit"),
(7, "Coman", "Matei", "1978-02-02", "740568542", "matei.coman@yahoo.com", "Iasi, Bd. Ion Mihalache nr. 10, et. 5, ap. 42", "Iasi, Bd. Ion Mihalache nr. 10, et. 5, ap. 42", "necasatorit"),
(8, "Moldovan", "Alina", "1981-11-26", "744985786", "alinam1990@yahoo.com", "Constanta, Bd. Mamaia nr. 5, ap. 10", "Constanta, Bd. Mamaia nr. 5, ap. 10", "necasatorit"),
(9, "Stupe", "Mariana", "1987-03-13", "746897417", "mariana.cj@gmail.com", "Bucuresti, str. Traian, nr. 10, bl. AB, ap. 7", "Bucuresti, str. Traian, nr. 10, bl. AB, ap. 7", "divortat"),
(10, "Urs", "Clara", "1997-12-01", "721362968", "clara_urss@yahoo.com", "Brasov, aleea Dorna, nr. 4", "Cluj-Napoca, aleea Vidraru nr. 5", "casatorit"),
(11, "Voevod", "Vasile", "2001-07-08", "745852845", "vvoevod@gmail.com", "Braila, str. Crisan, nr. 3", "Braila, str. Crisan, nr. 3", "casatorit");

>insert into clienti
values
(1, "Albu", "Ioana", "1981-11-17", "741912526", "ioana_albu@gmail.com", "Iasi splaiul Independentei nr. 35", "Iasi splaiul Independentei nr. 35", "casatorit");

>insert into produse_cont_curent
values
(1, 1, 4500.00, "2024-01-05", 5, null),
(2, 2, 230.50, "2023-03-31", 5, 1000.00),
(3, 3, 1099.26, "2017-11-25", 5, 5000.00),
(4, 4, 10.00, "2019-12-12", 10, 3000.00),
(5, 5, -185.00, "2023-02-12", 10, 2050),
(6, 6, 670.89, "2023-08-25", 5, null),
(7, 7, 9900.00, "2018-07-01", 5, 1000),
(8, 8, 121.25, "2017-11-25", 10, 1500),
(9, 9, 0.50, "2022-04-07", 5, null),
(10, 10, 121.00, "2022-12-23", 10, 900),
(11, 11, -1670.00, "2023-06-03", 10, 3000);

>insert into produse_creditare
values
(1, 1, 50000, "2023-10-01", 60, 44600.15, null, null, null, null),
(2, 3, null, null, null, null, 350000, "2017-12-08", 240, 270230.27);

>insert into produse_creditare
values
(3, 4, 20000, "2022-07-12", 48, 27052.07, 230000, "2022-08-03", 300, 211050),
(4, 6, 49000, "2023-08-31", 60, 44100, null, null, null, null),
(5, 9, 9000, "2024-03-05", 60, 8100, 310000, "2023-09-03", 180, 295000);

>insert into produse_economisire
values
(1, 2, "ct_economii_ron", "2023-05-01", 500, 1000),
(2, 3, "ct_depo_ron", "2019-10-01", 1000, 6559),
(3, 5, "ct_economii_eur", "2024-01-07", 200, 205.35),
(4, 6, "ct_depo_ron", "2024-02-15", 15000, 15125),
(5, 8, "ct_depo_eur", "2020-09-01", 10000, 10185),
(6, 10, "ct_economii_eur", "2022-12-16", 1000, 1025),
(7, 1, "ct_depo_ron", "2024-03-12", 15000, 15000);

>insert into chestionar_client_opinion
values
(1, 1, "2024-05-01", 10, "Am primit toate explicatiile de care aveam nevoie, sunt foarte multumit de banca si produse"),
(2, 3, "2024-05-25", 9, "Am asteptat putin pana ce am ajuns la ghiseu, dar odata ce am putut vorbi cu cineva din agentie totul s-a rezolvat repede"),
(3, 4, "2024-06-23", 4, "Am vrut sa iau un credit pentru o masina, dar mi-au spus ca nu ma incadrez, fara nici o alta explicatie"),
(4, 5, "2024-06-02", 2, "Nu sunt deloc multumit, aplicatia de internet banking functioneaza foarte greu si ma delogheaza inainte sa reusesc sa fac operatiunea"),
(5, 7, "2024-05-03", 10, "toate produsele sunt foarte potrivite pentru mine, colegii din agentie ma ajuta cand am nevoie"),
(6, 8, "2024-06-021", 8, "Am reusit sa accesez un credit imobiliar, dar s-au solicitat foarte multe documente"),
(7, 10, "2024-06-10", 7, "imi place aplicatia dar de multe ori ATM-urile nu functioneaza si trebuie sa umbli prin tot orasul ca sa retragi 100 de lei");

>insert into produse_creditare
values
(6, 11, null, null, null, null, 200000, "2023-11-05", 180, 189000);

>insert into produse_economisire values (8, 3, "ct_economii_ron", "2021-11-12", 300, 900);

After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:

>update clienti set stare_civila="divortat" where id_client=7;

>update produse_creditare set suma_acordata_NP=5500 where ID=2;

>update produse_creditare set data_acordarii_NP="2023-10-11" where ID=2;

>update produse_creditare set Perioada_NP_luni=60 where ID=2;

>update produse_creditare set suma_OUTST_NP=4500 where ID=2;

>update produse_creditare set suma_acordata_NP=11000, data_acordarii_NP="2024-02-26", perioada_NP_luni=24, suma_outst_NP=8700 where ID=6;

 >update clienti set adresa_email="andrei1980@gmail.com" where ID_client=5;
 
 
 ##### DQL(Data Query Language)
 
 In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:
 
 >select * from clienti where stare_civila="divortat";

>select nume, prenume, data_nasterii from clienti where stare_civila="necasatorit";

>select * from produse_economisire where suma_initiala>=10000;

>select id_client, nume, prenume from clienti where adresa_domiciliu like adresa_corespondenta;

>select * from clienti where adresa_email like "%gmail%";

>select id_client, data_chestionar from chestionar_client_opinion where nota <5;

>select * from produse_economisire where suma_initiala<500 or suma_curenta<500;

>select * from produse_economisire where suma_initiala<=500 or suma_curenta<=500;

>select * from chestionar_client_opinion;
select * from clienti;
select * from produse_cont_curent;
select * from produse_creditare;
select * from produse_economisire;

>select id_client, nume, prenume from clienti where adresa_domiciliu!=adresa_corespondenta;

>select id_client, nume, prenume from clienti where adresa_domiciliu=adresa_corespondenta;

>select id_client, data_desch_cont from produse_cont_curent 
where data_desch_cont between "2022-01-01" and "2023-12-31" 
and comision_lunar=5;

>select sum(suma_initiala) as sold_initial
 from produse_economisire;
 
 >select id_client, data_acordarii_NP 
 from produse_creditare
 where suma_outst_NP=(select max(Suma_OUTST_NP) from produse_creditare);
 
>select id_client, produs, suma_curenta
 from produse_economisire
 where suma_curenta=(select min(Suma_curenta) from produse_economisire); 
 
 >select sum(cont_crt_ron_sold) from produse_cont_curent;
 
 >select avg(cont_crt_ron_sold) from produse_cont_curent;
 
 >select count(*) from chestionar_client_opinion
 where Data_chestionar like "2024-06%";
 
> select count(*) from produse_creditare
 where perioada_imob_luni > 180;
 
> insert into produse_economisire values (8, 3, "ct_economii_ron", "2021-11-12", 300, 900);
  
> select nume, prenume, adresa_email from clienti
 cross join produse_economisire on clienti.id_client=produse_economisire.ID_Client;
 
> update clienti set adresa_email="andrei1980@gmail.com" where ID_client=5;
 
> select * from clienti 
 cross join produse_creditare on clienti.id_client=produse_creditare.ID_Client;
 
 >select nume, prenume, adresa_email from clienti
 inner join produse_economisire on clienti.id_client=produse_economisire.ID_Client;
 
 
> select * from clienti
 right join produse_creditare on clienti.id_client=produse_creditare.id_client;
 
 > select * from chestionar_client_opinion order by nota desc;
 
>select * from produse_economisire 
left join clienti on clienti.id_client=produse_economisire.id_client
order by suma_curenta desc limit 3; 
 
> select * from clienti
 left join produse_creditare on clienti.id_client=produse_creditare.id_client where Suma_acordata_NP is not null or Suma_acordata_IMOB is not null;
 
> select * from produse_creditare 
left join clienti on clienti.id_client=produse_creditare.id_client
order by data_acordarii_np limit 2; 
 
 As a conclusion, there are several reasons why SQl is important for data management and data analysis: it is a relatively easy to use way to acces, manipulate and extract data, it is efficient and can be integrated with various programming languages, tools, and platforms, making it a versatile solution for data management and analysis.