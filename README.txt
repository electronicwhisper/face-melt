uniform float time;
uniform float strength;

uniform sampler2D img0;
uniform sampler2D img1;
uniform sampler2D img2;
uniform sampler2D img3;
uniform sampler2D img4;
uniform sampler2D img5;
uniform sampler2D img6;
uniform sampler2D img7;
uniform sampler2D img8;
uniform sampler2D img9;

float wave(float angle, float phase) {
  vec2 p = gl_TexCoord[0].xy;
	p = (p-.5) * 40.;
	
	vec2 dir = vec2(cos(angle), sin(angle));
	float d = dot(p, dir);
	return cos(d+phase);
}

float b(float i) {
	float t = time * .008;
	float a = wave(i*t, t*100. + i);
	
	return smoothstep(0., 1., a);
}

void main() {
	vec2 p = gl_TexCoord[0].xy;
	
	vec4 col = texture2D(img0, p) * .05;
	
	col += b(0.) * texture2D(img0, p);
	col += b(1.) * texture2D(img1, p);
	col += b(2.) * texture2D(img2, p);
	col += b(3.) * texture2D(img3, p);
	col += b(4.) * texture2D(img4, p);
	col += b(5.) * texture2D(img5, p);
	col += b(6.) * texture2D(img6, p);
	col += b(7.) * texture2D(img7, p);
	
	col /= col.a;
	
	// blend with original face based on strength
	col = mix(texture2D(img0, p), col, strength);
	
	gl_FragColor = col;
}
