show databases;
show tables from company_constraints;
use company_constraints;

-- drop view employees_salary_view;
# Criação de View da tabela employee para busca de salario
create view employees_salary_view as 
	select concat(Fname, ' ', Minit, ' ', Lname) as Name, Salary as Salario, Dno as Numero_Departamento from employee
	where Salary > 6999
    order by Salary;

select * from employees_salary_view;

# Criação de View da tabela employee para busca do sexo 'M' 
create view employees_sex_view as 
	select concat(Fname, ' ', Minit, ' ', Lname) as Name, Salary, Dno as Dept_number from employee
	where Sex ='M';
    
select * from employees_sex_view;

# Criação de View da tabela dependent e employee     
select * from Dependent;
create or replace view dependent_view as 
	select concat(Fname, ' ', Minit, ' ', Lname) as Name, e.Dno as Departament, d.Dependent_name as Nome_dependente,
           d.Sex as Sexo_dependete, d.Bdate as Data_Nasc_Dependente, e.Ssn as Ssn_employee, d.Essn as Essn_dependente,
           Relationship  as Relação      
    from employee e
    inner join dependent d on e.Ssn = d.Essn;

select * from dependent_view;

-- Atualização de Salario;
update employees_salary_view
	set Numero_Departamento = 2 
    where Salario = 36800.00;
select * from employees_salary_view;

update employees_salary_view
	set Numero_Departamento = 5 
    where Name = 'Flanklin T WONG';

select * from employees_salary_view;

select * from dependent_view;

update dependent_view
	set Nome_dependente = 'John' and Name = 'Abners Father'
    where Departament = 4;
    
select * from employee;    
alter table company_constraints.employee add index employee_index (Ssn);
-- create index indicador on company_constraints.employee (Ssn);
    
create table employee_index(
Fname varchar(15) Not null,
minit char,
Lname varchar(15) Not null,
Ssn char(9) not null,
Bdate date,
Address varchar(30),
sex char,
Salary decimal(10,2),
Super_ssn char(9),
Dno int not null
) engine = MEMORY;
    create index idx_hash_Ssn on company_constraints.employee_index (Ssn) using hash;
    create index idx_btree_Ssn on company_constraints.employee_index (Ssn) using btree;
    
    show index from company_constraints.employee_index; 
    
    
  
create table employee(
Fname varchar(15) Not null,
minit char,
Lname varchar(15) Not null,
Ssn char(9) not null,
Bdate date,
Address varchar(30),
sex char,
Salary decimal(10,2),
Super_ssn char(9),
Dno int not null,
constraint chk_salary check (salary>200.00),
constraint pk_employee primary key (Ssn)
);


 drop procedure procedure_employee;
 select * from employee;

delimiter \\
create procedure procedure_employee(
    in Fname_p varchar(20),
    in Ssn_p varchar(30),
    in Salary_p varchar(60)
)
begin
	if (Salary_p >  6000.00) then 
		insert into user (Fname, Ssn, Salary) values(Fname_p, Ssn_p, Salary_p);
        select * from employee;
	else
		select 'Dados inconsistentes' as Message_error;
	end if;
end \\
delimiter ;   

call procedure_employee('Marcia','023456789', '30000.00');

