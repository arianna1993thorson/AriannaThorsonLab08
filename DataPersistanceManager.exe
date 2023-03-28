using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;

public class DataPersistanceManager : MonoBehaviour
{
    //public static DataPersistanceManager manager;

    [Header("File Storage Config")]
    [SerializeField] private string fileName;

    private List<IDataPersistence> dataPersistenceObjects;
    [SerializeField] private bool useEncryption;

    public static DataPersistanceManager manager { get; private set; }

    private FileHandler dataHandler;
    private GameData gameData;
    private List<IDataPersistence> dataPersistentObjects;

    private void Awake() {
        if(manager == null)
        {
            manager = this;
        }
        else
        {
            Debug.Log("Found more than one Data Persistance Manager in the scene");
        }
        this.dataHandler = new FileHandler(Application.persistentDataPath, fileName, useEncryption);
        this.dataPersistenceObjects = FindAllDataPersistenceObject();
        LoadGame();
    }

    //private void Start() {
    //    this.dataHandler = new FileHandler(Application.persistentDataPath, fileName, useEncryption);
    //    this.dataPersistenceObjects = FindAllDataPersistenceObject();
    //    LoadGame();
    //}

    public void NewGame()
    {
        this.gameData = new GameData();
    }

    public void LoadGame()
    {
        this.gameData = dataHandler.Load();

        if(this.gameData == null)
        {
            Debug.Log("No data was found! Initializing data to defaults.");
            NewGame();
        }
        foreach(IDataPersistence dataPersistanceObj in dataPersistenceObjects)
        {
            dataPersistanceObj.LoadData(gameData);
        }

        Debug.Log("Loaded score = " + gameData.score);
    }

    public void SaveGame()
    {
        foreach(IDataPersistence dataPersistenceObj in dataPersistenceObjects)
        {
            dataHandler.Save(gameData);

        }
        dataHandler.Save(gameData);
       // Debug.Log("Saved score = " + gameData.score);
    }

    private void OnApplicationQuit() {
        SaveGame();
    }

    private void onApplicationQuit()
    {
        SaveGame();
    }

    private List<IDataPersistence> FindAllDataPersistenceObject()
    {
        IEnumerable<IDataPersistence> dataPersistencesObjects = FindObjectsOfType<MonoBehaviour>().OfType<IDataPersistence>();
        return new List<IDataPersistence>(dataPersistencesObjects);
    }
}
