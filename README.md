# Consulta-a-base-de-datos

import java.sql.Connection;

import java.sql.DriverManager;

import java.sql.PreparedStatement;

import java.sql.ResultSet;

import java.sql.SQLException;

import java.util.ArrayList;

import java.util.List;

public class MaterialOperations {
    private static final String DB_URL = "jdbc:mysql://localhost:3306/tu_base_de_datos";

    private static final String USER = "root";
    
    private static final String PASS = "2004";
    
    public static void insertMaterial(int id, double cantidad, String medida, String nombre) {
    try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS)) {
        String sql = "INSERT INTO Material (id, cantidad, medida, nombre) VALUES (?, ?, ?, ?)";
        PreparedStatement preparedStatement = conn.prepareStatement(sql);
        preparedStatement.setInt(1, id);
        preparedStatement.setDouble(2, cantidad);
        preparedStatement.setString(3, medida);
        preparedStatement.setString(4, nombre);
        preparedStatement.executeUpdate();
    } catch (SQLException e) {
        e.printStackTrace();
    }
}

public static void updateMaterial(int id, double cantidad, String medida, String nombre) {
    try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS)) {
        String sql = "UPDATE Material SET cantidad=?, medida=?, nombre=? WHERE id=?";
        PreparedStatement preparedStatement = conn.prepareStatement(sql);
        preparedStatement.setDouble(1, cantidad);
        preparedStatement.setString(2, medida);
        preparedStatement.setString(3, nombre);
        preparedStatement.setInt(4, id);
        preparedStatement.executeUpdate();
    } catch (SQLException e) {
        e.printStackTrace();
    }
}

public static void deleteMaterial(int id) {
    try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS)) {
        String sql = "DELETE FROM Material WHERE id=?";
        PreparedStatement preparedStatement = conn.prepareStatement(sql);
        preparedStatement.setInt(1, id);
        preparedStatement.executeUpdate();
    } catch (SQLException e) {
        e.printStackTrace();
    }
}

    
    public static List<Material> getMaterials() {
        List<Material> materials = new ArrayList<>();
        try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS)) {
            String sql = "SELECT * FROM Material";
            PreparedStatement preparedStatement = conn.prepareStatement(sql);
            ResultSet resultSet = preparedStatement.executeQuery();

            while (resultSet.next()) {
                int id = resultSet.getInt("id");
                double cantidad = resultSet.getDouble("cantidad");
                String medida = resultSet.getString("medida");
                String nombre = resultSet.getString("nombre");

                Material material = new Material(id, cantidad, medida, nombre);
                materials.add(material);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return materials;
    }

    public static void main(String[] args) {
        // Ejemplo de cómo obtener y mostrar la lista de materiales
        List<Material> materials = getMaterials();
        for (Material material : materials) {
            System.out.println(material); 
        }
    }
}

//clase material 
public class Material {
    private int id;
    private double cantidad;
    private String medida;
    private String nombre;

    public Material(int id, double cantidad, String medida, String nombre) {
        this.id = id;
        this.cantidad = cantidad;
        this.medida = medida;
        this.nombre = nombre;
    }


    @Override
    public String toString() {
        return "Material [id=" + id + ", cantidad=" + cantidad + ", medida=" + medida + ", nombre=" + nombre + "]";
    }
}
