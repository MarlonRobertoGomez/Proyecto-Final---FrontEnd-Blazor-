using System.Collections.Generic;
using System.Net.Http;
using System.Net.Http.Json;
using System.Threading.Tasks;
using TuProyectoBiblioteca.Shared.Models; // Asegúrate de usar el namespace correcto

namespace TuProyectoBiblioteca.Client.Services // Ajusta el namespace
{
    public class AutorService
    {
        private readonly HttpClient _httpClient;
        private readonly string _apiEndpoint = "api/Autores"; // Ajusta la ruta de tu API

        public AutorService(HttpClient httpClient)
        {
            _httpClient = httpClient;
        }

        public async Task<List<Autor>> GetAutores()
        {
            return await _httpClient.GetFromJsonAsync<List<Autor>>(_apiEndpoint);
        }

        public async Task<Autor> GetAutor(int id)
        {
            return await _httpClient.GetFromJsonAsync<Autor>($"{_apiEndpoint}/{id}");
        }

        public async Task<Autor> CreateAutor(Autor autor)
        {
            var response = await _httpClient.PostAsJsonAsync(_apiEndpoint, autor);
            if (response.IsSuccessStatusCode)
            {
                return await response.Content.ReadFromJsonAsync<Autor>();
            }
            else
            {
                return null; // O lanza una excepción, dependiendo de tu manejo de errores
            }
        }

        public async Task<Autor> UpdateAutor(int id, Autor autor)
        {
            var response = await _httpClient.PutAsJsonAsync($"{_apiEndpoint}/{id}", autor);
            if (response.IsSuccessStatusCode)
            {
                return await response.Content.ReadFromJsonAsync<Autor>();
            }
            else
            {
                return null; // O lanza una excepción
            }
        }

        public async Task<bool> DeleteAutor(int id)
        {
            var response = await _httpClient.DeleteAsync($"{_apiEndpoint}/{id}");
            return response.IsSuccessStatusCode;
        }
    }
}


using System.ComponentModel.DataAnnotations;

namespace TuProyectoBiblioteca.Shared.Models  // Ajusta el namespace a tu proyecto Blazor
{
    public class Categoria
    {
        public int CategoriaId { get; set; }

        [Required(ErrorMessage = "El Nombre de la Categoría es requerido.")]
        [MaxLength(100, ErrorMessage = "El Nombre no puede exceder los 100 caracteres.")]
        public string Nombre { get; set; }

        [MaxLength(200, ErrorMessage = "La Descripción no puede exceder los 200 caracteres.")]
        public string Descripcion { get; set; }

        //  No incluimos la propiedad de navegación LibrosCategorias por defecto en el FrontEnd.
        //  Si necesitas mostrar información de los libros, asegúrate de que el BackEnd la incluya.
    }
}


using System;
using System.ComponentModel.DataAnnotations;

namespace TuProyectoBiblioteca.Shared.Models  // Ajusta el namespace a tu proyecto Blazor
{
    public class DetallePrestamo
    {
        public int DetallePrestamoId { get; set; }

        public int PrestamoId { get; set; }
        public int LibroId { get; set; }

        [Required(ErrorMessage = "La Fecha de Devolución Esperada es requerida.")]
        public DateTime FechaDevolucionEsperada { get; set; }

        public DateTime? FechaDevolucionReal { get; set; }

        [Required(ErrorMessage = "El Estado del Detalle es requerido.")]
        [MaxLength(20, ErrorMessage = "El Estado del Detalle no puede exceder los 20 caracteres.")]
        public string EstadoDetalle { get; set; }

        [MaxLength(50, ErrorMessage = "La Copia del Libro no puede exceder los 50 caracteres.")]
        public string CopiaLibro { get; set; }
    }
}


using System.ComponentModel.DataAnnotations;

namespace TuProyectoBiblioteca.Shared.Models  // Ajusta el namespace a tu proyecto Blazor
{
    public class Editorial
    {
        public int EditorialId { get; set; }

        [Required(ErrorMessage = "El Nombre de la Editorial es requerido.")]
        [MaxLength(100, ErrorMessage = "El Nombre no puede exceder los 100 caracteres.")]
        public string Nombre { get; set; }

        [MaxLength(200, ErrorMessage = "La Dirección no puede exceder los 200 caracteres.")]
        public string Direccion { get; set; }

        [MaxLength(20, ErrorMessage = "El Teléfono no puede exceder los 20 caracteres.")]
        public string Telefono { get; set; }

        [EmailAddress(ErrorMessage = "El Correo electrónico no es válido.")]
        [MaxLength(150, ErrorMessage = "El Correo no puede exceder los 150 caracteres.")]
        public string Correo { get; set; }

        [Url(ErrorMessage = "El Sitio Web no es una URL válida.")]
        [MaxLength(150, ErrorMessage = "El Sitio Web no puede exceder los 150 caracteres.")]
        public string SitioWeb { get; set; }

        //  No incluimos la propiedad de navegación Libros por defecto en el FrontEnd.
        //  Si necesitas mostrar información de los libros, asegúrate de que el BackEnd la incluya.
    }
}


using System;
using System.ComponentModel.DataAnnotations;

namespace TuProyectoBiblioteca.Shared.Models  // Ajusta el namespace a tu proyecto Blazor
{
    public class Libro
    {
        public int LibroId { get; set; }

        [Required(ErrorMessage = "El Título es requerido.")]
        [MaxLength(200, ErrorMessage = "El Título no puede exceder los 200 caracteres.")]
        public string Titulo { get; set; }

        [Required(ErrorMessage = "El ISBN es requerido.")]
        [MaxLength(20, ErrorMessage = "El ISBN no puede exceder los 20 caracteres.")]
        public string ISBN { get; set; }

        public DateTime? FechaPublicacion { get; set; }

        [MaxLength(50, ErrorMessage = "El Género no puede exceder los 50 caracteres.")]
        public string Genero { get; set; }

        [MaxLength(int.MaxValue, ErrorMessage = "La Sinopsis es demasiado larga.")]
        public string Sinopsis { get; set; }

        public int AutorId { get; set; }

        //  No necesitamos la propiedad de navegación Autor en el FrontEnd por defecto.
        //  Si la necesitas para mostrar información del autor, asegúrate de que el BackEnd la incluya.
    }
}


using System;
using System.ComponentModel.DataAnnotations;

namespace TuProyectoBiblioteca.Shared.Models  // Ajusta el namespace a tu proyecto Blazor
{
    public class Prestamo
    {
        public int PrestamoId { get; set; }

        [Required(ErrorMessage = "La Fecha de Préstamo es requerida.")]
        public DateTime FechaPrestamo { get; set; }

        [Required(ErrorMessage = "La Fecha de Devolución Esperada es requerida.")]
        public DateTime FechaDevolucionEsperada { get; set; }

        public DateTime? FechaDevolucionReal { get; set; }

        [Required(ErrorMessage = "El Estado del Préstamo es requerido.")]
        [MaxLength(20, ErrorMessage = "El Estado del Préstamo no puede exceder los 20 caracteres.")]
        public string EstadoPrestamo { get; set; }

        public int LibroId { get; set; }
        public int UsuarioId { get; set; }

        //  No incluimos las propiedades de navegación Libro y Usuario por defecto en el FrontEnd.
        //  Si necesitas mostrar información del libro o el usuario, asegúrate de que el BackEnd las incluya.
    }
}


using System;
using System.ComponentModel.DataAnnotations;

namespace TuProyectoBiblioteca.Shared.Models  // Ajusta el namespace a tu proyecto Blazor
{
    public class Reserva
    {
        public int ReservaId { get; set; }

        [Required(ErrorMessage = "La Fecha de Reserva es requerida.")]
        public DateTime FechaReserva { get; set; }

        [Required(ErrorMessage = "La Fecha Disponible Estimada es requerida.")]
        public DateTime FechaDisponibleEstimada { get; set; }

        public DateTime? FechaNotificacion { get; set; }

        [Required(ErrorMessage = "El Estado de la Reserva es requerido.")]
        [MaxLength(20, ErrorMessage = "El Estado de la Reserva no puede exceder los 20 caracteres.")]
        public string EstadoReserva { get; set; }

        public int LibroId { get; set; }
        public int UsuarioId { get; set; }

        //  No incluimos las propiedades de navegación Libro y Usuario por defecto en el FrontEnd.
        //  Si necesitas mostrar información del libro o el usuario, asegúrate de que el BackEnd las incluya.
    }
}


using System.ComponentModel.DataAnnotations;

namespace TuProyectoBiblioteca.Shared.Models  // Ajusta el namespace a tu proyecto Blazor
{
    public class Rol
    {
        public int RolId { get; set; }

        [Required(ErrorMessage = "El Nombre del Rol es requerido.")]
        [MaxLength(50, ErrorMessage = "El Nombre no puede exceder los 50 caracteres.")]
        public string Nombre { get; set; }

        [MaxLength(200, ErrorMessage = "La Descripción no puede exceder los 200 caracteres.")]
        public string Descripcion { get; set; }

        //  No incluimos la propiedad de navegación UsuariosRoles por defecto en el FrontEnd.
        //  Si necesitas mostrar información de los usuarios, asegúrate de que el BackEnd la incluya.
    }
}


using System.ComponentModel.DataAnnotations;

namespace TuProyectoBiblioteca.Shared.Models  // Ajusta el namespace a tu proyecto Blazor
{
    public class Ubicacion
    {
        public int UbicacionId { get; set; }

        [Required(ErrorMessage = "La Sección es requerida.")]
        [MaxLength(50, ErrorMessage = "La Sección no puede exceder los 50 caracteres.")]
        public string Seccion { get; set; }

        [Required(ErrorMessage = "El Estante es requerido.")]
        [MaxLength(50, ErrorMessage = "El Estante no puede exceder los 50 caracteres.")]
        public string Estante { get; set; }

        [Required(ErrorMessage = "La Fila es requerida.")]
        [MaxLength(50, ErrorMessage = "La Fila no puede exceder los 50 caracteres.")]
        public string Fila { get; set; }

        [Required(ErrorMessage = "El Código de Ubicación es requerido.")]
        [MaxLength(100, ErrorMessage = "El Código de Ubicación no puede exceder los 100 caracteres.")]
        public string CodigoUbicacion { get; set; }

        //  No incluimos la propiedad de navegación Libros por defecto en el FrontEnd.
        //  Si necesitas mostrar información de los libros, asegúrate de que el BackEnd la incluya.
    }
}


using System;
using System.ComponentModel.DataAnnotations;

namespace TuProyectoBiblioteca.Shared.Models  // Ajusta el namespace a tu proyecto Blazor
{
    public class Usuario
    {
        public int UsuarioId { get; set; }

        [Required(ErrorMessage = "El Nombre de Usuario es requerido.")]
        [MaxLength(50, ErrorMessage = "El Nombre de Usuario no puede exceder los 50 caracteres.")]
        public string NombreUsuario { get; set; }

        [EmailAddress(ErrorMessage = "El Correo electrónico no es válido.")]
        [MaxLength(150, ErrorMessage = "El Correo electrónico no puede exceder los 150 caracteres.")]
        public string Correo { get; set; }

        [Required(ErrorMessage = "La Contraseña es requerida.")]
        [MaxLength(255, ErrorMessage = "La Contraseña no puede exceder los 255 caracteres.")]
        public string Contraseña { get; set; }

        [MaxLength(20, ErrorMessage = "El Teléfono no puede exceder los 20 caracteres.")]
        public string Telefono { get; set; }

        [MaxLength(200, ErrorMessage = "La Dirección no puede exceder los 200 caracteres.")]
        public string Direccion { get; set; }

        public DateTime FechaRegistro { get; set; }

        public bool Estado { get; set; }
    }
}
