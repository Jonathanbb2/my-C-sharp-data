# my-C-sharp-data
c#

using System;
using System.Data;
using MySql.Data.MySqlClient;

namespace Data
{
    public class Program
    {
        static void Main(string[] args)
        {
            string server = "localhost";
            string port = "3306";
            string databaseName = "ragonworkers";
            string userName = "root";
            string password = "Jonathan2!";

            string connectionString = $"Server={server};Port={port};Database={databaseName};UID={userName};Password={password};";

            using MySqlConnection connection = new MySqlConnection(connectionString);

            try
            {
                connection.Open();
                Console.WriteLine("Connected to the database!");

                DataTable titlesData = GetTitlesData(connection);
                if (titlesData != null)
                {
                    foreach (DataRow row in titlesData.Rows)
                    {
                        Console.WriteLine($"Title ID: {row["TITLE_ID"]}, Worker Ref ID: {row["WORKER_REF_ID"]}, " +
                            $"Worker Title: {row["WORKER_TITLE"]}, Affected From: {row["AFFECTED_FROM"]}");
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Error while connecting to the database: " + ex.Message);
            }
            finally
            {
                if (connection.State == ConnectionState.Open)
                    connection.Close();
            }
        }

        public static DataTable GetTitlesData(MySqlConnection connection)
        {
            DataTable dataTable = new DataTable();
            string query = "SELECT * FROM TITLE";

            try
            {
                using MySqlCommand cmd = new MySqlCommand(query, connection);
                using MySqlDataAdapter adapter = new MySqlDataAdapter(cmd);
                adapter.Fill(dataTable);
            }
            catch (Exception ex)
            {
                Console.WriteLine("Error while fetching data: " + ex.Message);
            }

            return dataTable;
        }
    }
}
