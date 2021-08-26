# Triggers
TRIGGER
--Que disminuya la existencia de un repuesto utilizado en mantenimiento de equipos--
go
Create or alter trigger Debitar_Stock
   on mantenimiento
   for Insert
   as
   set nocount on
   Update repuestos  set repuestos.stock = repuestos.stock - inserted.CantidadRepuesto from inserted
     inner join repuestos on codigo= inserted.id_repuestopk
   go

CURSOR 
--Que muestre la cantidad de dinero invertido en todos los mantenimientos, y las utilidades--

declare  @totalcosto char(12),@totalServicio char(12),@totalrepuesto char(12),
@utilidades char(12),@total char(12),@totalMantenimiento char(12)
declare cursor8 cursor 
for select 
			sum (m.costo) as totalcosto,
			sum(t.precios) as totalServicio ,
			sum(c.Precio ) as totalRepuesto,
			sum(m.costo+t.precios-c.Precio) as utilidades,
			count(id_mantenimiento) as TotalMantenimiento,
			count(id_capacitaciones) as total 
			
from  mantenimiento as  m join tipo_servicio as t  on m.id_tipoServiciopk=t.id_tipoServicio join compra_repuesto as c on c.codigopk=m.id_repuestopk join capacitaciones as b on b.id_tecnicopk=m.id_tecnicopk 
open cursor8
fetch cursor8 into  @totalcosto ,@totalServicio ,@totalrepuesto ,@utilidades ,@total,@totalMantenimiento 
print '  ,DineroIngresado ,totalServicio ,totalInvertido ,Utilidades, totalCapacitaciones  totalMantenimientoDado '
print'------------------------------------------------------------------------------------------------------------'
while @@FETCH_STATUS=0
begin
	print @totalcosto +space(5)+@totalServicio +space(5)+@totalrepuesto +space(5)+
@utilidades  +space(5)+@total+space(5)+@totalMantenimiento 
	fetch cursor8 into  @totalcosto ,@totalServicio ,@totalrepuesto ,
@utilidades ,@total,@totalMantenimiento 
	end
	close cursor8
	deallocate cursor8

PROCEDIMIENTO ALMACENADO
--ingrese una marca y que me muestre cuantos mantenimiento le ha dado a tal marca-

create procedure servicios4
@marca varchar(50) as
select s.marca,s.caracteristica_equipo_Ingresado,n.descripcion_mante as MantenimientoEquipos,s.fecha_salida,n.estado_final  from mantenimiento n
 inner join solicitud_servicios s on s.id_servicios=n.id_serviciopk
where s.marca=@marca
select count(m.id_serviciopk) as totalMantenimientoMarca from solicitud_servicios s
inner join mantenimiento m on s.id_servicios= m.id_serviciopk where s.marca=@marca

go
exec servicios4 'Kia'
