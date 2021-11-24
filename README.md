# Politica-de-limpeza-de-tabelas-no-MySQL-Event-Job-
Exemplo de politica criada no MySql que fica limpando a tabela a cada 5 minutos


```sql
-- VERIFICA SE O EVENTO ESTA HABILITADO
select @@event_scheduler;

-- HABILITA EVENTO
set global event_scheduler = ON;

-- DESABILITA EVENTO
set global event_scheduler = OFF;

-- EXIBE OS EVENTOS
SHOW EVENTS; 

-- EXIBE O FONTE DO EVENTO
show create event LIMPEZA_TABELAS_DETRO;

-- EXCLUI EVENTO
drop event LIMPEZA_TABELAS_DETRO;

-- CRIA EVENTO
delimiter |
alter EVENT LIMPEZA_TABELAS_DETRO
     ON SCHEDULE EVERY 5 minute
    COMMENT 'APAGA MENSAGENS MANTENDO APENAS 2 MESES DE INFORMACAO'
    DO
      BEGIN
         DELETE FROM detro.MENSAGEMENVIADA WHERE DATAHORA <  DATE_SUB(current_timestamp(),INTERVAL 1 MONTH) LIMIT 30000;
		 DELETE FROM detro.mensagemrespondida WHERE DATAHORA <  DATE_SUB(current_timestamp(),INTERVAL 1 MONTH) LIMIT 30000;
		
		
      END|

delimiter ;
```
