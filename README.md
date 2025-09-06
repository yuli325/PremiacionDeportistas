# PremiacionDeportistas
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;

namespace PremiacionDeportistas
{
    class Deportista
    {
        public string Nombre { get; set; }
        public string Disciplina { get; set; }
        public int Puntuacion { get; set; }

        public Deportista(string nombre, string disciplina, int puntuacion)
        {
            Nombre = nombre;
            Disciplina = disciplina;
            Puntuacion = puntuacion;
        }

        public override string ToString()
        {
            return $"{Nombre} - {Puntuacion} pts";
        }
    }

    class Program
    {
        static HashSet<string> deportistasRegistrados = new HashSet<string>();
        static Dictionary<string, List<Deportista>> disciplinas = new Dictionary<string, List<Deportista>>();

        static void RegistrarDeportista(string nombre, string disciplina, int puntuacion)
        {
            if (deportistasRegistrados.Contains(nombre))
            {
                Console.WriteLine("Advertencia: El deportista " + nombre + " ya fue registrado.");
                return;
            }

            deportistasRegistrados.Add(nombre);
            Deportista d = new Deportista(nombre, disciplina, puntuacion);

            if (!disciplinas.ContainsKey(disciplina))
            {
                disciplinas[disciplina] = new List<Deportista>();
            }

            disciplinas[disciplina].Add(d);
            Console.WriteLine("Deportista " + nombre + " registrado en " + disciplina + " con " + puntuacion + " puntos.");
        }

        static void Premiacion()
        {
            Console.WriteLine("\nPremiacion por disciplina:");

            foreach (var entry in disciplinas)
            {
                Console.WriteLine("\n" + entry.Key + ":");

                var top3 = entry.Value
                    .OrderByDescending(d => d.Puntuacion)
                    .Take(3)
                    .ToList();

                for (int i = 0; i < top3.Count; i++)
                {
                    Console.WriteLine("  " + (i + 1) + ". " + top3[i].ToString());
                }
            }
        }

        static void Reporte()
        {
            Console.WriteLine("\nReporte de disciplinas y deportistas:");

            foreach (var entry in disciplinas)
            {
                Console.WriteLine("- " + entry.Key + ": " + entry.Value.Count + " deportistas");
            }
        }

        static void Main(string[] args)
        {
            Stopwatch tiempo = new Stopwatch();
            tiempo.Start();

            RegistrarDeportista("Ana", "Natacion", 85);
            RegistrarDeportista("Luis", "Atletismo", 90);
            RegistrarDeportista("Ana", "Natacion", 88); // Duplicado
            RegistrarDeportista("Carlos", "Atletismo", 78);
            RegistrarDeportista("Sofia", "Natacion", 92);
            RegistrarDeportista("Marta", "Natacion", 88);
            RegistrarDeportista("Pedro", "Atletismo", 81);

            tiempo.Stop();

            Premiacion();
            Reporte();

            Console.WriteLine("\nTiempo de ejecucion: " + tiempo.Elapsed.TotalMilliseconds + " ms");
            Console.ReadLine();
        }
    }
}
