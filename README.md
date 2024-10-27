using System;
using System.Collections.Generic;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Drawing.Imaging;
using System.IO;

namespace PresentationMaker
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Добро пожаловать в Presentation Maker!");

            // Создаем новую презентацию
            Presentation presentation = new Presentation();

            // Добавляем слайды
            presentation.AddSlide("Слайд 1", "Заголовок слайда 1", "Описание слайда 1");
            presentation.AddSlide("Слайд 2", "Заголовок слайда 2", "Описание слайда 2");

            // Сохраняем презентацию в виде изображений
            presentation.SaveAsImages("output");

            Console.WriteLine("Презентация создана успешно!");
            Console.ReadKey();
        }
    }

    // Класс для представления презентации
    class Presentation
    {
        private List<Slide> slides;

        public Presentation()
        {
            slides = new List<Slide>();
        }

        // Добавляем новый слайд
        public void AddSlide(string title, string header, string description)
        {
            slides.Add(new Slide(title, header, description));
        }

        // Сохраняем презентацию в виде изображений
        public void SaveAsImages(string outputFolder)
        {
            // Создаем папку для вывода, если ее нет
            if (!Directory.Exists(outputFolder))
            {
                Directory.CreateDirectory(outputFolder);
            }

            // Создаем изображения для каждого слайда
            for (int i = 0; i < slides.Count; i++)
            {
                string imagePath = Path.Combine(outputFolder, $"slide_{i + 1}.png");
                slides[i].SaveAsImage(imagePath);
            }
        }
    }

    // Класс для представления слайда
    class Slide
    {
        private string title;
        private string header;
        private string description;

        public Slide(string title, string header, string description)
        {
            this.title = title;
            this.header = header;
            this.description = description;
        }

        // Сохраняем слайд в виде изображения
        public void SaveAsImage(string imagePath)
        {
            // Создаем изображение
            Bitmap image = new Bitmap(800, 600);
            Graphics g = Graphics.FromImage(image);
            g.SmoothingMode = SmoothingMode.AntiAlias;

            // Задаем фон
            g.FillRectangle(Brushes.White, 0, 0, image.Width, image.Height);

            // Рисуем заголовок
            Font headerFont = new Font("Arial", 36, FontStyle.Bold);
            SizeF headerSize = g.MeasureString(header, headerFont);
            g.DrawString(header, headerFont, Brushes.Black, (image.Width - headerSize.Width) / 2, 50);

            // Рисуем описание
            Font descriptionFont = new Font("Arial", 18);
            SizeF descriptionSize = g.MeasureString(description, descriptionFont);
            g.DrawString(description, descriptionFont, Brushes.Black, (image.Width - descriptionSize.Width) / 2, 150);

            // Рисуем заголовок слайда
            Font titleFont = new Font("Arial", 14);
            SizeF titleSize = g.MeasureString(title, titleFont);
            g.DrawString(title, titleFont, Brushes.Gray, (image.Width - titleSize.Width) / 2, image.Height - 50);

            // Сохраняем изображение
            image.Save(imagePath, ImageFormat.Png);
        }
    }
}
