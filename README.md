# GPUGems3_DigitalEmily2_Unity


![リンクテキスト](https://forum.unity3d.com/attachments/digitalemily_unity_01-jpg.209692/"タイトル")





- http://gl.ict.usc.edu/Research/DigitalEmily2/



'''
float phongExponent(float glossiness) {
	return (1/pow(1-glossiness, 3.5)-1);
}

surface emily_material
(
  string specularUnlitNormTexture="00_specular_unlit.exr",
  string singleScatterTexture="00_single_scatter.exr",
  string diffuseUnlitTexture="00_diffuse_unlit.exr",
  float scatterRadius=0.5,
  color scatterColor=color(0.482, 0.169, 0.109),
  float ior=1.33,
  float phaseFunction=0.8
)
{
	color diffuseUnlit=texture(diffuseUnlitTexture, u, v);
	color singleScatter=texture(singleScatterTexture, u, v);
	color specularUnlit=texture(specularUnlitNormTexture, u, v);

	// Single scattering is approximated with a diffuse closure
	color diffuseAmount=singleScatter;

	// Multiple scattering reduced with the single scattering amount
	color sssAmount=(color(1,1,1)-singleScatter)*diffuseUnlit;

	// We have two Phong specular lobes with equal strength, so each is half as bright
	color specularAmount=specularUnlit*0.5;

	// Fresnel coefficient; this should really be glossy Fresnel that takes into
	// account the specular roughness, but for the moment we are just using the
	// perfect mirror Fresnel.
	float fresnelCoeff, refractionStrength;
	vector reflectDir, refractDir;
	fresnel(I, N, ior, fresnelCoeff, refractionStrength, reflectDir, refractDir);

	specularAmount*=fresnelCoeff;

	// Compute the final result.
 	Ci=
 		specularAmount*phong(N, phongExponent(0.55))+
 		specularAmount*phong(N, phongExponent(0.75))+
 		diffuseAmount*diffuse(N)+
 		subsurface(ior, phaseFunction, scatterColor*scatterRadius, sssAmount);
}
'''





'''
git -c diff.mnemonicprefix=false -c core.quotepath=false push -v --tags origin master:master
POST git-receive-pack (chunked)

remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.        
remote: error: Trace: 1973b852787f85c301a24bfa2dce52bd        

remote: error: See http://git.io/iEPt8g for more information.        
remote: error: File Assets/Textures/Color_raw/00_single_scatter_raw.exr is 104.09 MB; this exceeds GitHub's file size limit of 100.00 MB        



Pushing to https://github.com/whaison/GPUGems3_DigitalEmily2_Unity.git
To https://github.com/whaison/GPUGems3_DigitalEmily2_Unity.git
 ! [remote rejected] master -> master (pre-receive hook declined)



error: failed to push some refs to 'https://github.com/whaison/GPUGems3_DigitalEmily2_Unity.git'

エラー終了しました。エラーの内容は上記をご覧ください。

'''
