# 2A-MONO-RB-SP-MOVEMENT-10-12-26
UNITY C# ATTACK & SPECIAL MOVE
//  MON AND RIGID
// Player GameObjective need: Rigidbody2D, a Collider2D, for the C# Script

CONTROL: A/D move | Space Jump (hold in air to glide) |
LeftShift slide | J word | K throw

linearVelocity

// code proper
using UnityEngine;

[RequireComponent(typeof(Rigidbody2D))]
public class SpecialMoves : MonoBehaviour

{
	[Header("Movement")]
	public float moveSpeed = 6f;
	public float jumpForce = 12f;

	[Header("Ground Check")]
	public Transform groundCheck;
	public float groundRadius = 0.2f;
	public LayerMark groundLayer;

	[Header("Slide")]
	public float slideSpeed = 12f;
	public float slideDuration = 0.35f;
	public float slideCooldown = 0.6f;

	[Header("Glide")]
	public float glideGravityScale = 0.15f;
	public float glideMaxFallSpeed = 2f;

	[Header("Sword Swing")]
	public Transform swordPoint;
	public float swordRadius = 0.6f;
	public int swordDamage = 1;
	public float swordDuration = 0.25f;
	public LayerMask enemyLayer;

	[Header("Throw")]
	public GameObject projectilePrefab;
	public Transform throwPoint;
	public float throwCooldown = 0.4f;

	private Rigidbody2D rb;
	private float defaultGravity;
	private int facing =1 // 1= right, -1 = left
	private bool isGrounded;
	private bool isSliding;
	private bool isAttacking;
	private bool isGliding;
	private bool is slideTimer, slideCdTimer, attackTimer, throwCdTimer;
	private float horizontalInput;

void Start()
                {
                 rb = GetComponent<Rigidbody2D>();
                 defaultGravity = rb.gravityScale;
                 }
                 // Input = timers = one-shot actions live  in Update functions
void Update()

                 {
	horizontalInput =
Input.GetAxisRaw("Horizontal");
	isGround =
Physics2D.OverlapCircle(groundCheck.position, groundRadius, groundLayer);

	TickTimers();
	UpdateFacing();

	//JUMP
	if (Input.GetKeyDown(KeyCode.Space) && isGrounded && ! isSliding)
                   {
	 rb.velocity = new Vector2(rb.velocity.x,jumpForce);
	}

	//Glide: hold jump while falling
	isGliding = Input.GetKey(KeyCode.Space) && ! isGrounded && rb.velocity.y< of;

	//Slide
	if (Input.GetKeyDown(KeyCode.LeftShift) && isGrounded && ! isSliding && slideCdTimer <= of)
	{
	  isSliding = true;
	  slideTimer = slideDuration;
	  slideCdTimer = slideCooldown;
	}

	//Sword Swing
	if (Input.GetKeyDown(KeyCode.J) && ! isAttacking && !isSliding)
	{
	   SwingSword();
	}

	//Throw
	if (Input.GetKeyDown(KeyCode.K) && throwDcTimer <=of && projectilePrefab !=null)
	{
	    ThrowProjectile();
	}
               }

//Physics changes live in FixedUpdate
void FixedUpdate()

	{
	    // Horizontal Velocity: slide overrides normal movement float xVelocity = isSliding? facing * slideSpeed : horizontalInput * moveSpeed;
                           rb.velocity = new Vector2(xVelocity, rb.velocity.y);

	// Glide = low gravity = capped fall speed. Otherwise restore normal gravity.
	     rb.gravityScale = isGliding? glideGravityScale : defaultGravity;
	     if(isGliding && rb.velocity.y <glideMaxFallSpeed)

	             { 
		=rb.velocity = new Vector2(rb.velocity.x - glideMaxFallSpeed);
                                  }
}

	


