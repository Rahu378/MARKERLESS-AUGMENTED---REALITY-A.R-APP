# MARKERLESS-AUGMENTED---REALITY-A.R-APP

using UnityEngine;
using System.Collections;
using UnityEngine.UI;

public class webCamScript : MonoBehaviour {

  public GameObject webCameraPlane; 
  public Button fireButton;


  // Use this for initialization
  void Start () {

    if (Application.isMobilePlatform) {
      GameObject cameraParent = new GameObject ("camParent");
      cameraParent.transform.position = this.transform.position;
      this.transform.parent = cameraParent.transform;
      cameraParent.transform.Rotate (Vector3.right, 90);
    }

    Input.gyro.enabled = true;

    fireButton.onClick.AddListener (OnButtonDown);


    WebCamTexture webCameraTexture = new WebCamTexture();
    webCameraPlane.GetComponent<MeshRenderer>().material.mainTexture = webCameraTexture;
    webCameraTexture.Play();




  }


  void OnButtonDown(){

    GameObject bullet = Instantiate(Resources.Load("bullet", typeof(GameObject))) as GameObject;
    Rigidbody rb = bullet.GetComponent<Rigidbody>();
    bullet.transform.rotation = Camera.main.transform.rotation;
    bullet.transform.position = Camera.main.transform.position;
    rb.AddForce(Camera.main.transform.forward * 500f);
    Destroy (bullet, 3);

    GetComponent<AudioSource> ().Play ();


  }
  
  // Update is called once per frame
  void Update () {

    Quaternion cameraRotation = new Quaternion (Input.gyro.attitude.x, Input.gyro.attitude.y, -Input.gyro.attitude.z, -Input.gyro.attitude.w);
    this.transform.localRotation = cameraRotation;
  
  }
}



using UnityEngine;
using System.Collections;

public class enemyScript : MonoBehaviour {



  // Use this for initialization
  void Start () {

    StartCoroutine ("Move");
  }

  // Update is called once per frame
  void Update () {

    transform.Translate(Vector3.forward * 3f * Time.deltaTime); 
  }

  IEnumerator Move() {


    while (true) {
      yield return new WaitForSeconds (3.5f);
      transform.eulerAngles += new Vector3 (0, 180f, 0);
    }
  }
}




using UnityEngine;
using System.Collections;

public class collisionScript : MonoBehaviour {

  // Use this for initialization
  void Start () {

  }

  // Update is called once per frame
  void Update () {

  }

  //for this to work both need colliders, one must have rigid body (spaceship) the other must have is trigger checked.
  void OnTriggerEnter (Collider col)
  {
    GameObject explosion = Instantiate(Resources.Load("FlareMobile", typeof(GameObject))) as GameObject;
    explosion.transform.position = transform.position;
    Destroy(col.gameObject);
    Destroy (explosion, 2);


    if (GameObject.FindGameObjectsWithTag("Player").Length == 0){

      GameObject enemy = Instantiate(Resources.Load("enemy", typeof(GameObject))) as GameObject;
      GameObject enemy1 = Instantiate(Resources.Load("enemy1", typeof(GameObject))) as GameObject;
      GameObject enemy2 = Instantiate(Resources.Load("enemy2", typeof(GameObject))) as GameObject;
      GameObject enemy3 = Instantiate(Resources.Load("enemy3", typeof(GameObject))) as GameObject;

    }

    Destroy (gameObject);


  }

}






